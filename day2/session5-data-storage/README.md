# Session 5: Data & Storage (3 ชั่วโมง)

## 🎯 วัตถุประสงค์
เรียนรู้การจัดเก็บและจัดการข้อมูลใน Android App

## 📚 เนื้อหา

### 1. SharedPreferences (45 นาที)

SharedPreferences ใช้เก็บข้อมูลแบบ Key-Value (เหมาะกับข้อมูลง่ายๆ เช่น settings)

#### การใช้งาน:

```kotlin
class PreferencesActivity : AppCompatActivity() {

    private lateinit var sharedPreferences: SharedPreferences

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_preferences)

        // Initialize SharedPreferences
        sharedPreferences = getSharedPreferences("MyPrefs", Context.MODE_PRIVATE)

        // บันทึกข้อมูล
        saveData()

        // อ่านข้อมูล
        loadData()
    }

    private fun saveData() {
        val editor = sharedPreferences.edit()
        
        editor.putString("username", "somkiat")
        editor.putInt("age", 25)
        editor.putBoolean("isLoggedIn", true)
        editor.putFloat("score", 95.5f)
        
        // บันทึก (async)
        editor.apply()
        
        // หรือ editor.commit() สำหรับ sync
    }

    private fun loadData() {
        val username = sharedPreferences.getString("username", "default_user")
        val age = sharedPreferences.getInt("age", 0)
        val isLoggedIn = sharedPreferences.getBoolean("isLoggedIn", false)
        val score = sharedPreferences.getFloat("score", 0f)

        Log.d("Prefs", "Username: $username, Age: $age, LoggedIn: $isLoggedIn")
    }

    private fun clearData() {
        sharedPreferences.edit().clear().apply()
    }

    private fun removeKey() {
        sharedPreferences.edit().remove("username").apply()
    }
}
```

#### ตัวอย่างการใช้งานจริง: Remember Me

```kotlin
class LoginActivity : AppCompatActivity() {

    private lateinit var prefs: SharedPreferences
    private lateinit var editUsername: EditText
    private lateinit var editPassword: EditText
    private lateinit var checkRemember: CheckBox

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_login)

        prefs = getSharedPreferences("LoginPrefs", Context.MODE_PRIVATE)

        editUsername = findViewById(R.id.editUsername)
        editPassword = findViewById(R.id.editPassword)
        checkRemember = findViewById(R.id.checkRemember)

        // โหลดข้อมูลที่บันทึกไว้
        loadSavedCredentials()

        findViewById<Button>(R.id.btnLogin).setOnClickListener {
            login()
        }
    }

    private fun loadSavedCredentials() {
        val savedUsername = prefs.getString("username", "")
        val rememberMe = prefs.getBoolean("remember_me", false)

        if (rememberMe && !savedUsername.isNullOrEmpty()) {
            editUsername.setText(savedUsername)
            checkRemember.isChecked = true
        }
    }

    private fun login() {
        val username = editUsername.text.toString()
        val password = editPassword.text.toString()

        // Validate and login...

        // บันทึกถ้าเลือก Remember Me
        if (checkRemember.isChecked) {
            prefs.edit()
                .putString("username", username)
                .putBoolean("remember_me", true)
                .apply()
        } else {
            prefs.edit()
                .remove("username")
                .putBoolean("remember_me", false)
                .apply()
        }
    }
}
```

### 2. Data Classes และ Lists (30 นาที)

```kotlin
// Data Class
data class Task(
    val id: Int,
    val title: String,
    val description: String,
    val isCompleted: Boolean = false,
    val createdAt: Long = System.currentTimeMillis()
)

// การใช้งาน
val task1 = Task(1, "เรียน Kotlin", "เรียนคอร์ส 2 วัน", false)
val task2 = task1.copy(isCompleted = true)

// List Operations
val tasks = mutableListOf<Task>()

// Add
tasks.add(Task(1, "งาน 1", "รายละเอียด 1"))
tasks.add(Task(2, "งาน 2", "รายละเอียด 2"))

// Get
val firstTask = tasks[0]
val lastTask = tasks.last()

// Update
tasks[0] = tasks[0].copy(isCompleted = true)

// Remove
tasks.removeAt(0)

// Filter
val completedTasks = tasks.filter { it.isCompleted }
val pendingTasks = tasks.filter { !it.isCompleted }

// Sort
val sortedByTitle = tasks.sortedBy { it.title }
val sortedByDate = tasks.sortedByDescending { it.createdAt }
```

### 3. RecyclerView (75 นาที)

RecyclerView ใช้แสดงรายการข้อมูลจำนวนมากอย่างมีประสิทธิภาพ

#### Step 1: เพิ่ม Dependency

**build.gradle.kts**
```kotlin
dependencies {
    implementation("androidx.recyclerview:recyclerview:1.3.2")
}
```

#### Step 2: สร้าง Layout สำหรับ Item

**item_task.xml**
```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.cardview.widget.CardView
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:layout_margin="8dp"
    app:cardCornerRadius="8dp"
    app:cardElevation="4dp">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        android:padding="16dp">

        <CheckBox
            android:id="@+id/checkBox"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content" />

        <LinearLayout
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:orientation="vertical"
            android:layout_marginStart="12dp">

            <TextView
                android:id="@+id/tvTitle"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:text="Task Title"
                android:textSize="18sp"
                android:textStyle="bold"
                android:textColor="#000000" />

            <TextView
                android:id="@+id/tvDescription"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:text="Task Description"
                android:textSize="14sp"
                android:textColor="#666666"
                android:layout_marginTop="4dp" />

        </LinearLayout>

        <ImageButton
            android:id="@+id/btnDelete"
            android:layout_width="48dp"
            android:layout_height="48dp"
            android:src="@android:drawable/ic_menu_delete"
            android:background="?attr/selectableItemBackgroundBorderless"
            android:contentDescription="Delete" />

    </LinearLayout>

</androidx.cardview.widget.CardView>
```

#### Step 3: สร้าง Adapter

**TaskAdapter.kt**
```kotlin
class TaskAdapter(
    private val tasks: MutableList<Task>,
    private val onTaskClick: (Task) -> Unit,
    private val onTaskDelete: (Task) -> Unit,
    private val onTaskCheck: (Task, Boolean) -> Unit
) : RecyclerView.Adapter<TaskAdapter.TaskViewHolder>() {

    inner class TaskViewHolder(itemView: View) : RecyclerView.ViewHolder(itemView) {
        val checkBox: CheckBox = itemView.findViewById(R.id.checkBox)
        val tvTitle: TextView = itemView.findViewById(R.id.tvTitle)
        val tvDescription: TextView = itemView.findViewById(R.id.tvDescription)
        val btnDelete: ImageButton = itemView.findViewById(R.id.btnDelete)
    }

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): TaskViewHolder {
        val view = LayoutInflater.from(parent.context)
            .inflate(R.layout.item_task, parent, false)
        return TaskViewHolder(view)
    }

    override fun onBindViewHolder(holder: TaskViewHolder, position: Int) {
        val task = tasks[position]

        holder.tvTitle.text = task.title
        holder.tvDescription.text = task.description
        holder.checkBox.isChecked = task.isCompleted

        // Click listeners
        holder.itemView.setOnClickListener {
            onTaskClick(task)
        }

        holder.checkBox.setOnCheckedChangeListener { _, isChecked ->
            onTaskCheck(task, isChecked)
        }

        holder.btnDelete.setOnClickListener {
            onTaskDelete(task)
        }

        // Strike through if completed
        if (task.isCompleted) {
            holder.tvTitle.paintFlags = holder.tvTitle.paintFlags or Paint.STRIKE_THRU_TEXT_FLAG
            holder.tvTitle.alpha = 0.5f
        } else {
            holder.tvTitle.paintFlags = holder.tvTitle.paintFlags and Paint.STRIKE_THRU_TEXT_FLAG.inv()
            holder.tvTitle.alpha = 1.0f
        }
    }

    override fun getItemCount() = tasks.size

    fun addTask(task: Task) {
        tasks.add(task)
        notifyItemInserted(tasks.size - 1)
    }

    fun removeTask(task: Task) {
        val position = tasks.indexOf(task)
        if (position != -1) {
            tasks.removeAt(position)
            notifyItemRemoved(position)
        }
    }

    fun updateTask(task: Task) {
        val position = tasks.indexOfFirst { it.id == task.id }
        if (position != -1) {
            tasks[position] = task
            notifyItemChanged(position)
        }
    }
}
```

#### Step 4: ใช้งาน RecyclerView

**activity_todo.xml**
```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <androidx.recyclerview.widget.RecyclerView
        android:id="@+id/recyclerView"
        android:layout_width="0dp"
        android:layout_height="0dp"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintBottom_toTopOf="@id/layoutInput"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent" />

    <LinearLayout
        android:id="@+id/layoutInput"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        android:padding="16dp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent">

        <EditText
            android:id="@+id/editTask"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:hint="เพิ่มงานใหม่..."
            android:inputType="text" />

        <Button
            android:id="@+id/btnAdd"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="เพิ่ม"
            android:layout_marginStart="8dp" />

    </LinearLayout>

</androidx.constraintlayout.widget.ConstraintLayout>
```

**TodoActivity.kt**
```kotlin
class TodoActivity : AppCompatActivity() {

    private lateinit var recyclerView: RecyclerView
    private lateinit var adapter: TaskAdapter
    private lateinit var editTask: EditText
    private lateinit var btnAdd: Button
    
    private val tasks = mutableListOf<Task>()
    private var nextId = 1

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_todo)

        recyclerView = findViewById(R.id.recyclerView)
        editTask = findViewById(R.id.editTask)
        btnAdd = findViewById(R.id.btnAdd)

        setupRecyclerView()
        setupListeners()
        loadTasks()
    }

    private fun setupRecyclerView() {
        adapter = TaskAdapter(
            tasks = tasks,
            onTaskClick = { task ->
                // Edit task
                showEditDialog(task)
            },
            onTaskDelete = { task ->
                deleteTask(task)
            },
            onTaskCheck = { task, isChecked ->
                updateTask(task.copy(isCompleted = isChecked))
            }
        )

        recyclerView.adapter = adapter
        recyclerView.layoutManager = LinearLayoutManager(this)
        
        // Add divider
        val divider = DividerItemDecoration(this, DividerItemDecoration.VERTICAL)
        recyclerView.addItemDecoration(divider)
    }

    private fun setupListeners() {
        btnAdd.setOnClickListener {
            addTask()
        }

        editTask.setOnEditorActionListener { _, actionId, _ ->
            if (actionId == EditorInfo.IME_ACTION_DONE) {
                addTask()
                true
            } else {
                false
            }
        }
    }

    private fun addTask() {
        val title = editTask.text.toString().trim()
        if (title.isNotEmpty()) {
            val task = Task(
                id = nextId++,
                title = title,
                description = "",
                isCompleted = false
            )
            adapter.addTask(task)
            editTask.text.clear()
            saveTasks()
            
            // Scroll to bottom
            recyclerView.smoothScrollToPosition(tasks.size - 1)
        }
    }

    private fun updateTask(task: Task) {
        adapter.updateTask(task)
        saveTasks()
    }

    private fun deleteTask(task: Task) {
        adapter.removeTask(task)
        saveTasks()
        Toast.makeText(this, "ลบงานแล้ว", Toast.LENGTH_SHORT).show()
    }

    private fun showEditDialog(task: Task) {
        val builder = AlertDialog.Builder(this)
        val input = EditText(this)
        input.setText(task.title)

        builder.setTitle("แก้ไขงาน")
            .setView(input)
            .setPositiveButton("บันทึก") { _, _ ->
                val newTitle = input.text.toString().trim()
                if (newTitle.isNotEmpty()) {
                    updateTask(task.copy(title = newTitle))
                }
            }
            .setNegativeButton("ยกเลิก", null)
            .show()
    }

    private fun saveTasks() {
        val prefs = getSharedPreferences("TodoPrefs", Context.MODE_PRIVATE)
        val gson = Gson()
        val json = gson.toJson(tasks)
        prefs.edit().putString("tasks", json).apply()
    }

    private fun loadTasks() {
        val prefs = getSharedPreferences("TodoPrefs", Context.MODE_PRIVATE)
        val json = prefs.getString("tasks", null)
        if (json != null) {
            val gson = Gson()
            val type = object : TypeToken<MutableList<Task>>() {}.type
            val loadedTasks = gson.fromJson<MutableList<Task>>(json, type)
            tasks.clear()
            tasks.addAll(loadedTasks)
            nextId = (tasks.maxOfOrNull { it.id } ?: 0) + 1
            adapter.notifyDataSetChanged()
        }
    }
}
```

## 💻 แบบฝึกหัด: Todo List App

สร้างแอพ To-Do List ที่มีฟีเจอร์:
- ✅ เพิ่มงานใหม่
- ✅ แสดงรายการงาน
- ✅ ทำเครื่องหมายงานเสร็จ
- ✅ ลบงาน
- ✅ แก้ไขงาน
- ✅ บันทึกข้อมูลด้วย SharedPreferences

## 📝 เฉลย

เฉลยแบบฝึกหัดอยู่ที่: [solutions/session5-solutions/](../../solutions/session5-solutions/)

## 🔗 Resources

- [SharedPreferences Guide](https://developer.android.com/training/data-storage/shared-preferences)
- [RecyclerView Guide](https://developer.android.com/guide/topics/ui/layout/recyclerview)

## ⏭️ Next Session

[Session 6: Networking & APIs](../session6-networking/README.md)
