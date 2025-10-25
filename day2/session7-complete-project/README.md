# Session 7: Complete Project (3 ชั่วโมง)

## 🎯 วัตถุประสงค์
สร้างแอพที่สมบูรณ์โดยรวมความรู้ทั้งหมดที่ได้เรียนมา

## 📱 โปรเจค: TaskMaster - Todo App with API

แอพจัดการงานที่มีฟีเจอร์ครบถ้วน:
- ✅ CRUD Operations (Create, Read, Update, Delete)
- ✅ Local Storage (Room Database)
- ✅ API Integration
- ✅ MVVM Architecture
- ✅ Material Design UI
- ✅ Navigation Components

## 🏗️ Architecture: MVVM

```
┌─────────────────────────────────────┐
│            View (Activity)          │
│  - UI Components                    │
│  - User Interactions                │
└──────────────┬──────────────────────┘
               │ observes
               ▼
┌─────────────────────────────────────┐
│          ViewModel                  │
│  - UI State                         │
│  - Business Logic                   │
│  - LiveData/StateFlow               │
└──────────────┬──────────────────────┘
               │ calls
               ▼
┌─────────────────────────────────────┐
│          Repository                 │
│  - Data Source Coordinator          │
│  - Cache Management                 │
└──────┬──────────────────────┬───────┘
       │                      │
       ▼                      ▼
┌─────────────┐      ┌─────────────┐
│   Local DB  │      │  Remote API │
│   (Room)    │      │  (Retrofit) │
└─────────────┘      └─────────────┘
```

## 📂 Project Structure

```
TaskMaster/
├── data/
│   ├── local/
│   │   ├── TaskDao.kt
│   │   ├── TaskDatabase.kt
│   │   └── TaskEntity.kt
│   ├── remote/
│   │   ├── ApiService.kt
│   │   └── RetrofitClient.kt
│   └── repository/
│       └── TaskRepository.kt
├── domain/
│   └── model/
│       └── Task.kt
├── ui/
│   ├── main/
│   │   ├── MainActivity.kt
│   │   ├── MainViewModel.kt
│   │   └── TaskAdapter.kt
│   ├── add/
│   │   ├── AddTaskActivity.kt
│   │   └── AddTaskViewModel.kt
│   └── detail/
│       ├── TaskDetailActivity.kt
│       └── TaskDetailViewModel.kt
└── utils/
    ├── Resource.kt
    └── Extensions.kt
```

## 📝 Implementation

### 1. Dependencies

**build.gradle.kts**
```kotlin
plugins {
    id("com.android.application")
    id("org.jetbrains.kotlin.android")
    id("kotlin-kapt")
}

android {
    namespace = "com.example.taskmaster"
    compileSdk = 34

    defaultConfig {
        applicationId = "com.example.taskmaster"
        minSdk = 24
        targetSdk = 34
        versionCode = 1
        versionName = "1.0"
    }

    buildFeatures {
        viewBinding = true
    }
}

dependencies {
    // Core
    implementation("androidx.core:core-ktx:1.12.0")
    implementation("androidx.appcompat:appcompat:1.6.1")
    implementation("com.google.android.material:material:1.10.0")
    implementation("androidx.constraintlayout:constraintlayout:2.1.4")

    // Lifecycle
    implementation("androidx.lifecycle:lifecycle-viewmodel-ktx:2.6.2")
    implementation("androidx.lifecycle:lifecycle-livedata-ktx:2.6.2")
    implementation("androidx.lifecycle:lifecycle-runtime-ktx:2.6.2")

    // Room
    implementation("androidx.room:room-runtime:2.6.0")
    implementation("androidx.room:room-ktx:2.6.0")
    kapt("androidx.room:room-compiler:2.6.0")

    // Retrofit
    implementation("com.squareup.retrofit2:retrofit:2.9.0")
    implementation("com.squareup.retrofit2:converter-gson:2.9.0")
    implementation("com.squareup.okhttp3:logging-interceptor:4.11.0")

    // Coroutines
    implementation("org.jetbrains.kotlinx:kotlinx-coroutines-android:1.7.3")

    // Navigation
    implementation("androidx.navigation:navigation-fragment-ktx:2.7.5")
    implementation("androidx.navigation:navigation-ui-ktx:2.7.5")
}
```

### 2. Data Layer

#### TaskEntity.kt
```kotlin
@Entity(tableName = "tasks")
data class TaskEntity(
    @PrimaryKey(autoGenerate = true)
    val id: Int = 0,
    val title: String,
    val description: String,
    val isCompleted: Boolean = false,
    val priority: Int = 0, // 0=Low, 1=Medium, 2=High
    val dueDate: Long? = null,
    val createdAt: Long = System.currentTimeMillis(),
    val updatedAt: Long = System.currentTimeMillis()
)
```

#### TaskDao.kt
```kotlin
@Dao
interface TaskDao {
    
    @Query("SELECT * FROM tasks ORDER BY createdAt DESC")
    fun getAllTasks(): Flow<List<TaskEntity>>
    
    @Query("SELECT * FROM tasks WHERE id = :taskId")
    suspend fun getTaskById(taskId: Int): TaskEntity?
    
    @Query("SELECT * FROM tasks WHERE isCompleted = :isCompleted ORDER BY createdAt DESC")
    fun getTasksByStatus(isCompleted: Boolean): Flow<List<TaskEntity>>
    
    @Insert(onConflict = OnConflictStrategy.REPLACE)
    suspend fun insertTask(task: TaskEntity): Long
    
    @Update
    suspend fun updateTask(task: TaskEntity)
    
    @Delete
    suspend fun deleteTask(task: TaskEntity)
    
    @Query("DELETE FROM tasks WHERE id = :taskId")
    suspend fun deleteTaskById(taskId: Int)
    
    @Query("DELETE FROM tasks")
    suspend fun deleteAllTasks()
}
```

#### TaskDatabase.kt
```kotlin
@Database(
    entities = [TaskEntity::class],
    version = 1,
    exportSchema = false
)
abstract class TaskDatabase : RoomDatabase() {
    
    abstract fun taskDao(): TaskDao
    
    companion object {
        @Volatile
        private var INSTANCE: TaskDatabase? = null
        
        fun getDatabase(context: Context): TaskDatabase {
            return INSTANCE ?: synchronized(this) {
                val instance = Room.databaseBuilder(
                    context.applicationContext,
                    TaskDatabase::class.java,
                    "task_database"
                ).build()
                INSTANCE = instance
                instance
            }
        }
    }
}
```

#### TaskRepository.kt
```kotlin
class TaskRepository(private val taskDao: TaskDao) {
    
    val allTasks: Flow<List<TaskEntity>> = taskDao.getAllTasks()
    
    fun getTasksByStatus(isCompleted: Boolean): Flow<List<TaskEntity>> {
        return taskDao.getTasksByStatus(isCompleted)
    }
    
    suspend fun getTaskById(taskId: Int): TaskEntity? {
        return taskDao.getTaskById(taskId)
    }
    
    suspend fun insertTask(task: TaskEntity): Long {
        return taskDao.insertTask(task)
    }
    
    suspend fun updateTask(task: TaskEntity) {
        taskDao.updateTask(task.copy(updatedAt = System.currentTimeMillis()))
    }
    
    suspend fun deleteTask(task: TaskEntity) {
        taskDao.deleteTask(task)
    }
    
    suspend fun toggleTaskCompletion(task: TaskEntity) {
        val updatedTask = task.copy(
            isCompleted = !task.isCompleted,
            updatedAt = System.currentTimeMillis()
        )
        taskDao.updateTask(updatedTask)
    }
}
```

### 3. ViewModel Layer

#### MainViewModel.kt
```kotlin
class MainViewModel(application: Application) : AndroidViewModel(application) {
    
    private val repository: TaskRepository
    val allTasks: LiveData<List<TaskEntity>>
    
    init {
        val taskDao = TaskDatabase.getDatabase(application).taskDao()
        repository = TaskRepository(taskDao)
        allTasks = repository.allTasks.asLiveData()
    }
    
    fun insertTask(task: TaskEntity) = viewModelScope.launch {
        repository.insertTask(task)
    }
    
    fun updateTask(task: TaskEntity) = viewModelScope.launch {
        repository.updateTask(task)
    }
    
    fun deleteTask(task: TaskEntity) = viewModelScope.launch {
        repository.deleteTask(task)
    }
    
    fun toggleTaskCompletion(task: TaskEntity) = viewModelScope.launch {
        repository.toggleTaskCompletion(task)
    }
    
    fun getTasksByStatus(isCompleted: Boolean): LiveData<List<TaskEntity>> {
        return repository.getTasksByStatus(isCompleted).asLiveData()
    }
}
```

### 4. UI Layer

#### activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.coordinatorlayout.widget.CoordinatorLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <com.google.android.material.appbar.AppBarLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content">

        <com.google.android.material.appbar.MaterialToolbar
            android:id="@+id/toolbar"
            android:layout_width="match_parent"
            android:layout_height="?attr/actionBarSize"
            android:background="?attr/colorPrimary"
            app:title="TaskMaster"
            app:titleTextColor="@android:color/white" />

    </com.google.android.material.appbar.AppBarLayout>

    <androidx.constraintlayout.widget.ConstraintLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:layout_behavior="@string/appbar_scrolling_view_behavior">

        <!-- Tabs -->
        <com.google.android.material.tabs.TabLayout
            android:id="@+id/tabLayout"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            app:layout_constraintTop_toTopOf="parent"
            app:layout_constraintStart_toStartOf="parent"
            app:layout_constraintEnd_toEndOf="parent">

            <com.google.android.material.tabs.TabItem
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="ทั้งหมด" />

            <com.google.android.material.tabs.TabItem
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="รอดำเนินการ" />

            <com.google.android.material.tabs.TabItem
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="เสร็จแล้ว" />

        </com.google.android.material.tabs.TabLayout>

        <!-- RecyclerView -->
        <androidx.recyclerview.widget.RecyclerView
            android:id="@+id/recyclerView"
            android:layout_width="0dp"
            android:layout_height="0dp"
            app:layout_constraintTop_toBottomOf="@id/tabLayout"
            app:layout_constraintBottom_toBottomOf="parent"
            app:layout_constraintStart_toStartOf="parent"
            app:layout_constraintEnd_toEndOf="parent"
            android:padding="8dp"
            android:clipToPadding="false" />

        <!-- Empty State -->
        <LinearLayout
            android:id="@+id/layoutEmpty"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:orientation="vertical"
            android:gravity="center"
            android:visibility="gone"
            app:layout_constraintTop_toTopOf="parent"
            app:layout_constraintBottom_toBottomOf="parent"
            app:layout_constraintStart_toStartOf="parent"
            app:layout_constraintEnd_toEndOf="parent">

            <ImageView
                android:layout_width="120dp"
                android:layout_height="120dp"
                android:src="@drawable/ic_empty"
                android:alpha="0.3"
                android:contentDescription="Empty" />

            <TextView
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="ไม่มีงาน"
                android:textSize="18sp"
                android:textColor="#999999"
                android:layout_marginTop="16dp" />

        </LinearLayout>

    </androidx.constraintlayout.widget.ConstraintLayout>

    <!-- FAB -->
    <com.google.android.material.floatingactionbutton.FloatingActionButton
        android:id="@+id/fabAdd"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="bottom|end"
        android:layout_margin="16dp"
        android:src="@drawable/ic_add"
        android:contentDescription="Add Task"
        app:tint="@android:color/white" />

</androidx.coordinatorlayout.widget.CoordinatorLayout>
```

#### MainActivity.kt
```kotlin
class MainActivity : AppCompatActivity() {

    private lateinit var binding: ActivityMainBinding
    private lateinit var viewModel: MainViewModel
    private lateinit var adapter: TaskAdapter
    
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        binding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(binding.root)
        
        setupToolbar()
        setupViewModel()
        setupRecyclerView()
        setupTabLayout()
        setupFab()
        observeTasks()
    }
    
    private fun setupToolbar() {
        setSupportActionBar(binding.toolbar)
    }
    
    private fun setupViewModel() {
        viewModel = ViewModelProvider(this)[MainViewModel::class.java]
    }
    
    private fun setupRecyclerView() {
        adapter = TaskAdapter(
            onTaskClick = { task ->
                // Open task detail
                openTaskDetail(task)
            },
            onTaskCheck = { task, isChecked ->
                viewModel.toggleTaskCompletion(task)
            },
            onTaskDelete = { task ->
                showDeleteConfirmation(task)
            }
        )
        
        binding.recyclerView.adapter = adapter
        binding.recyclerView.layoutManager = LinearLayoutManager(this)
        
        // Add item decoration
        val divider = DividerItemDecoration(this, DividerItemDecoration.VERTICAL)
        binding.recyclerView.addItemDecoration(divider)
    }
    
    private fun setupTabLayout() {
        binding.tabLayout.addOnTabSelectedListener(object : TabLayout.OnTabSelectedListener {
            override fun onTabSelected(tab: TabLayout.Tab?) {
                when (tab?.position) {
                    0 -> observeAllTasks()
                    1 -> observePendingTasks()
                    2 -> observeCompletedTasks()
                }
            }
            
            override fun onTabUnselected(tab: TabLayout.Tab?) {}
            override fun onTabReselected(tab: TabLayout.Tab?) {}
        })
    }
    
    private fun setupFab() {
        binding.fabAdd.setOnClickListener {
            openAddTask()
        }
    }
    
    private fun observeTasks() {
        observeAllTasks()
    }
    
    private fun observeAllTasks() {
        viewModel.allTasks.observe(this) { tasks ->
            updateUI(tasks)
        }
    }
    
    private fun observePendingTasks() {
        viewModel.getTasksByStatus(false).observe(this) { tasks ->
            updateUI(tasks)
        }
    }
    
    private fun observeCompletedTasks() {
        viewModel.getTasksByStatus(true).observe(this) { tasks ->
            updateUI(tasks)
        }
    }
    
    private fun updateUI(tasks: List<TaskEntity>) {
        adapter.submitList(tasks)
        binding.layoutEmpty.visibility = if (tasks.isEmpty()) View.VISIBLE else View.GONE
    }
    
    private fun openAddTask() {
        val intent = Intent(this, AddTaskActivity::class.java)
        startActivity(intent)
    }
    
    private fun openTaskDetail(task: TaskEntity) {
        val intent = Intent(this, TaskDetailActivity::class.java)
        intent.putExtra("TASK_ID", task.id)
        startActivity(intent)
    }
    
    private fun showDeleteConfirmation(task: TaskEntity) {
        AlertDialog.Builder(this)
            .setTitle("ลบงาน")
            .setMessage("คุณต้องการลบงานนี้หรือไม่?")
            .setPositiveButton("ลบ") { _, _ ->
                viewModel.deleteTask(task)
                Toast.makeText(this, "ลบงานแล้ว", Toast.LENGTH_SHORT).show()
            }
            .setNegativeButton("ยกเลิก", null)
            .show()
    }
    
    override fun onCreateOptionsMenu(menu: Menu?): Boolean {
        menuInflater.inflate(R.menu.main_menu, menu)
        return true
    }
    
    override fun onOptionsItemSelected(item: MenuItem): Boolean {
        return when (item.itemId) {
            R.id.action_settings -> {
                // Open settings
                true
            }
            else -> super.onOptionsItemSelected(item)
        }
    }
}
```

## 💻 แบบฝึกหัด

### ฟีเจอร์ที่ต้องเพิ่ม:
1. ✅ AddTaskActivity - เพิ่มงานใหม่
2. ✅ TaskDetailActivity - แสดงและแก้ไขงาน
3. ✅ Search functionality - ค้นหางาน
4. ✅ Filter by priority - กรองตามความสำคัญ
5. ✅ Sort options - เรียงลำดับ
6. ✅ Notifications - แจ้งเตือนงาน

## 📝 Testing

- ทดสอบเพิ่ม/แก้ไข/ลบงาน
- ทดสอบ filter และ sort
- ทดสอบหมุนหน้าจอ
- ทดสอบ back press
- ทดสอบ empty state

## 🎨 UI/UX Tips

- ใช้ Material Design Components
- เพิ่ม animations และ transitions
- แสดง loading states
- Handle errors gracefully
- Accessibility support

## ⏭️ Next Session

[Session 8: Publishing](../session8-publishing/README.md)
