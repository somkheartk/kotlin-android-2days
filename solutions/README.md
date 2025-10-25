# เฉลยแบบฝึกหัด

## 📚 Session 1: Kotlin Basics

### Exercise 1: BMI Calculator

```kotlin
fun calculateBMI(weight: Double, height: Double): String {
    val bmi = weight / (height * height)
    
    return when {
        bmi < 18.5 -> "ผอม (BMI: %.2f)".format(bmi)
        bmi < 25.0 -> "ปกติ (BMI: %.2f)".format(bmi)
        bmi < 30.0 -> "อ้วน (BMI: %.2f)".format(bmi)
        else -> "อ้วนมาก (BMI: %.2f)".format(bmi)
    }
}

// ตัวอย่างการใช้งาน
fun main() {
    val result1 = calculateBMI(65.0, 1.70) // น้ำหนัก 65kg ส่วนสูง 170cm
    println(result1) // ปกติ (BMI: 22.49)
    
    val result2 = calculateBMI(50.0, 1.70)
    println(result2) // ผอม (BMI: 17.30)
    
    val result3 = calculateBMI(85.0, 1.70)
    println(result3) // อ้วน (BMI: 29.41)
}
```

### Exercise 2: Student Management

```kotlin
data class Student(
    val id: Int,
    val name: String,
    var score: Int
)

class StudentManager {
    private val students = mutableListOf<Student>()
    
    fun addStudent(student: Student) {
        students.add(student)
        println("เพิ่มนักเรียน: ${student.name}")
    }
    
    fun removeStudent(id: Int) {
        val student = students.find { it.id == id }
        if (student != null) {
            students.remove(student)
            println("ลบนักเรียน: ${student.name}")
        } else {
            println("ไม่พบนักเรียน ID: $id")
        }
    }
    
    fun getTopStudents(count: Int = 3): List<Student> {
        return students.sortedByDescending { it.score }.take(count)
    }
    
    fun getAverageScore(): Double {
        if (students.isEmpty()) return 0.0
        return students.map { it.score }.average()
    }
    
    fun displayAllStudents() {
        students.forEach { student ->
            println("ID: ${student.id}, Name: ${student.name}, Score: ${student.score}")
        }
    }
}

// ตัวอย่างการใช้งาน
fun main() {
    val manager = StudentManager()
    
    manager.addStudent(Student(1, "สมชาย", 85))
    manager.addStudent(Student(2, "สมหญิง", 92))
    manager.addStudent(Student(3, "สมศักดิ์", 78))
    manager.addStudent(Student(4, "สมใจ", 95))
    
    println("\nนักเรียนทั้งหมด:")
    manager.displayAllStudents()
    
    println("\nTop 3 นักเรียน:")
    manager.getTopStudents(3).forEach {
        println("${it.name}: ${it.score}")
    }
    
    println("\nคะแนนเฉลี่ย: %.2f".format(manager.getAverageScore()))
    
    manager.removeStudent(3)
}
```

### Exercise 3: Shopping Cart

```kotlin
data class Product(
    val id: Int,
    val name: String,
    val price: Double
)

class ShoppingCart {
    private val items = mutableMapOf<Product, Int>() // Product to Quantity
    
    fun addProduct(product: Product, quantity: Int = 1) {
        val currentQty = items[product] ?: 0
        items[product] = currentQty + quantity
        println("เพิ่ม ${product.name} x$quantity")
    }
    
    fun removeProduct(product: Product) {
        if (items.remove(product) != null) {
            println("ลบ ${product.name} ออกจากตะกร้า")
        } else {
            println("ไม่พบสินค้านี้ในตะกร้า")
        }
    }
    
    fun getTotalPrice(): Double {
        return items.entries.sumOf { (product, quantity) ->
            product.price * quantity
        }
    }
    
    fun applyDiscount(percentage: Double): Double {
        val total = getTotalPrice()
        val discount = total * (percentage / 100)
        return total - discount
    }
    
    fun displayCart() {
        println("\n=== ตะกร้าสินค้า ===")
        if (items.isEmpty()) {
            println("ตะกร้าว่างเปล่า")
            return
        }
        
        items.forEach { (product, quantity) ->
            val subtotal = product.price * quantity
            println("${product.name} x$quantity = ${"%.2f".format(subtotal)} บาท")
        }
        println("\nรวม: ${"%.2f".format(getTotalPrice())} บาท")
    }
}

// ตัวอย่างการใช้งาน
fun main() {
    val cart = ShoppingCart()
    
    val apple = Product(1, "แอปเปิ้ล", 50.0)
    val banana = Product(2, "กล้วย", 30.0)
    val orange = Product(3, "ส้ม", 40.0)
    
    cart.addProduct(apple, 3)
    cart.addProduct(banana, 5)
    cart.addProduct(orange, 2)
    
    cart.displayCart()
    
    println("\nหลังส่วนลด 10%: ${"%.2f".format(cart.applyDiscount(10.0))} บาท")
    
    cart.removeProduct(banana)
    cart.displayCart()
}
```

## 📱 Session 2-4: Android Development

### Counter App

```kotlin
class CounterActivity : AppCompatActivity() {
    private lateinit var tvCounter: TextView
    private lateinit var btnIncrement: Button
    private lateinit var btnDecrement: Button
    private lateinit var btnReset: Button
    
    private var counter = 0
    
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_counter)
        
        tvCounter = findViewById(R.id.tvCounter)
        btnIncrement = findViewById(R.id.btnIncrement)
        btnDecrement = findViewById(R.id.btnDecrement)
        btnReset = findViewById(R.id.btnReset)
        
        updateDisplay()
        
        btnIncrement.setOnClickListener {
            counter++
            updateDisplay()
        }
        
        btnDecrement.setOnClickListener {
            if (counter > 0) {
                counter--
                updateDisplay()
            }
        }
        
        btnReset.setOnClickListener {
            counter = 0
            updateDisplay()
            Toast.makeText(this, "Reset!", Toast.LENGTH_SHORT).show()
        }
    }
    
    private fun updateDisplay() {
        tvCounter.text = counter.toString()
        btnDecrement.isEnabled = counter > 0
    }
    
    override fun onSaveInstanceState(outState: Bundle) {
        super.onSaveInstanceState(outState)
        outState.putInt("counter", counter)
    }
    
    override fun onRestoreInstanceState(savedInstanceState: Bundle) {
        super.onRestoreInstanceState(savedInstanceState)
        counter = savedInstanceState.getInt("counter", 0)
        updateDisplay()
    }
}
```

### Login Screen

```kotlin
class LoginActivity : AppCompatActivity() {
    private lateinit var editEmail: EditText
    private lateinit var editPassword: EditText
    private lateinit var checkRemember: CheckBox
    private lateinit var btnLogin: Button
    
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_login)
        
        editEmail = findViewById(R.id.editEmail)
        editPassword = findViewById(R.id.editPassword)
        checkRemember = findViewById(R.id.checkRemember)
        btnLogin = findViewById(R.id.btnLogin)
        
        btnLogin.setOnClickListener {
            validateAndLogin()
        }
    }
    
    private fun validateAndLogin() {
        val email = editEmail.text.toString().trim()
        val password = editPassword.text.toString()
        
        // Validate email
        if (email.isEmpty()) {
            editEmail.error = "กรุณากรอกอีเมล"
            editEmail.requestFocus()
            return
        }
        
        if (!android.util.Patterns.EMAIL_ADDRESS.matcher(email).matches()) {
            editEmail.error = "รูปแบบอีเมลไม่ถูกต้อง"
            editEmail.requestFocus()
            return
        }
        
        // Validate password
        if (password.isEmpty()) {
            editPassword.error = "กรุณากรอกรหัสผ่าน"
            editPassword.requestFocus()
            return
        }
        
        if (password.length < 6) {
            editPassword.error = "รหัสผ่านต้องมีอย่างน้อย 6 ตัวอักษร"
            editPassword.requestFocus()
            return
        }
        
        // Login successful
        Toast.makeText(this, "เข้าสู่ระบบสำเร็จ!", Toast.LENGTH_SHORT).show()
        
        // Save to SharedPreferences if remember me is checked
        if (checkRemember.isChecked) {
            val prefs = getSharedPreferences("LoginPrefs", MODE_PRIVATE)
            prefs.edit()
                .putString("email", email)
                .putBoolean("remember", true)
                .apply()
        }
        
        // Navigate to main activity
        val intent = Intent(this, MainActivity::class.java)
        startActivity(intent)
        finish()
    }
}
```

## 📝 Session 5: Data & Storage

### Todo List with SharedPreferences

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
        loadTasks()
        
        btnAdd.setOnClickListener {
            addTask()
        }
    }
    
    private fun setupRecyclerView() {
        adapter = TaskAdapter(
            tasks = tasks,
            onTaskClick = { task -> editTask(task) },
            onTaskDelete = { task -> deleteTask(task) },
            onTaskCheck = { task, isChecked -> toggleTask(task, isChecked) }
        )
        
        recyclerView.adapter = adapter
        recyclerView.layoutManager = LinearLayoutManager(this)
    }
    
    private fun addTask() {
        val title = editTask.text.toString().trim()
        if (title.isNotEmpty()) {
            val task = Task(nextId++, title, "", false)
            adapter.addTask(task)
            editTask.text.clear()
            saveTasks()
        }
    }
    
    private fun editTask(task: Task) {
        // Show edit dialog
    }
    
    private fun deleteTask(task: Task) {
        adapter.removeTask(task)
        saveTasks()
    }
    
    private fun toggleTask(task: Task, isChecked: Boolean) {
        val updatedTask = task.copy(isCompleted = isChecked)
        adapter.updateTask(updatedTask)
        saveTasks()
    }
    
    private fun saveTasks() {
        val prefs = getSharedPreferences("TodoPrefs", MODE_PRIVATE)
        val gson = Gson()
        val json = gson.toJson(tasks)
        prefs.edit().putString("tasks", json).apply()
    }
    
    private fun loadTasks() {
        val prefs = getSharedPreferences("TodoPrefs", MODE_PRIVATE)
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

## 🔗 Additional Solutions

เฉลยโปรเจคเพิ่มเติมสามารถดูได้ใน:
- [Calculator App Solution](./calculator-solution/)
- [Weather App Solution](./weather-solution/)
- [TaskMaster Solution](./taskmaster-solution/)

---

**หมายเหตุ:** เฉลยเหล่านี้เป็นตัวอย่างหนึ่ง อาจมีวิธีแก้ปัญหาที่แตกต่างกันได้
