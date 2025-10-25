# Session 1: พื้นฐาน Kotlin (3 ชั่วโมง)

## 🎯 วัตถุประสงค์
เรียนรู้พื้นฐานภาษา Kotlin เพื่อเตรียมความพร้อมสำหรับการพัฒนา Android App

## 📚 เนื้อหา

### 1. แนะนำ Kotlin (30 นาที)
- Kotlin คืออะไร?
- ทำไมต้องใช้ Kotlin สำหรับ Android
- ความแตกต่างระหว่าง Kotlin กับ Java
- ติดตั้ง Kotlin Playground หรือ IntelliJ IDEA

### 2. ตัวแปรและชนิดข้อมูล (45 นาที)

#### ตัวแปร
```kotlin
// val = Immutable (ไม่สามารถเปลี่ยนแปลงได้)
val name: String = "Somkiat"
val age = 25 // Type inference

// var = Mutable (สามารถเปลี่ยนแปลงได้)
var score: Int = 100
score = 150 // OK
```

#### ชนิดข้อมูลพื้นฐาน
```kotlin
// Numbers
val intValue: Int = 42
val longValue: Long = 1234567890L
val floatValue: Float = 3.14f
val doubleValue: Double = 3.14159

// Boolean
val isActive: Boolean = true

// Characters & Strings
val char: Char = 'A'
val text: String = "Hello Kotlin"
val multiLine = """
    สวัสดี
    Kotlin
"""

// Nullable Types
var nullableText: String? = null
nullableText = "Now has value"
```

### 3. Operators (30 นาที)

```kotlin
// Arithmetic Operators
val sum = 10 + 5
val difference = 10 - 5
val product = 10 * 5
val quotient = 10 / 5
val remainder = 10 % 3

// Comparison Operators
val isEqual = (10 == 5)      // false
val isNotEqual = (10 != 5)   // true
val isGreater = (10 > 5)     // true

// Logical Operators
val and = true && false      // false
val or = true || false       // true
val not = !true              // false

// String Templates
val name = "Somkiat"
val greeting = "สวัสดี $name"
val lengthInfo = "ความยาว: ${name.length}"
```

### 4. Control Flow (45 นาที)

#### If Expression
```kotlin
val max = if (a > b) {
    println("a มากกว่า")
    a
} else {
    println("b มากกว่า")
    b
}

// One line
val result = if (score >= 50) "ผ่าน" else "ไม่ผ่าน"
```

#### When Expression
```kotlin
val grade = when (score) {
    in 80..100 -> "A"
    in 70..79 -> "B"
    in 60..69 -> "C"
    in 50..59 -> "D"
    else -> "F"
}

when {
    score >= 80 -> println("ดีมาก")
    score >= 50 -> println("ผ่าน")
    else -> println("ไม่ผ่าน")
}
```

#### Loops
```kotlin
// For Loop
for (i in 1..5) {
    println(i) // 1, 2, 3, 4, 5
}

for (i in 1 until 5) {
    println(i) // 1, 2, 3, 4
}

for (i in 5 downTo 1) {
    println(i) // 5, 4, 3, 2, 1
}

for (i in 0..10 step 2) {
    println(i) // 0, 2, 4, 6, 8, 10
}

// While Loop
var count = 0
while (count < 5) {
    println(count)
    count++
}

// Do-While Loop
do {
    println("ทำอย่างน้อย 1 ครั้ง")
} while (false)
```

### 5. Functions (45 นาที)

```kotlin
// Basic Function
fun greet(name: String): String {
    return "สวัสดี $name"
}

// Single Expression Function
fun add(a: Int, b: Int): Int = a + b

// Default Parameters
fun greetWithTitle(name: String, title: String = "คุณ"): String {
    return "สวัสดี$title$name"
}

// Named Arguments
greetWithTitle(title = "ครู", name = "สมเกียรติ")

// Varargs
fun sumAll(vararg numbers: Int): Int {
    return numbers.sum()
}
val total = sumAll(1, 2, 3, 4, 5)

// Lambda Functions
val multiply = { a: Int, b: Int -> a * b }
val result = multiply(5, 3) // 15

// Higher Order Functions
fun calculate(a: Int, b: Int, operation: (Int, Int) -> Int): Int {
    return operation(a, b)
}

val sum = calculate(5, 3) { x, y -> x + y }
val product = calculate(5, 3) { x, y -> x * y }
```

### 6. Collections (30 นาที)

```kotlin
// Lists
val numbers = listOf(1, 2, 3, 4, 5)          // Immutable
val mutableNumbers = mutableListOf(1, 2, 3)   // Mutable
mutableNumbers.add(4)

// Sets
val uniqueNumbers = setOf(1, 2, 3, 2, 1)      // {1, 2, 3}
val mutableSet = mutableSetOf<String>()

// Maps
val ages = mapOf(
    "Somkiat" to 25,
    "Sompong" to 30
)
val mutableAges = mutableMapOf<String, Int>()
mutableAges["Somchai"] = 28

// Collection Operations
val filtered = numbers.filter { it > 2 }       // [3, 4, 5]
val mapped = numbers.map { it * 2 }            // [2, 4, 6, 8, 10]
val summed = numbers.sum()                     // 15
val first = numbers.first()                    // 1
val last = numbers.last()                      // 5
```

### 7. Classes และ Objects (45 นาที)

```kotlin
// Basic Class
class Person(val name: String, var age: Int) {
    fun introduce() {
        println("สวัสดี ผม$name อายุ $age ปี")
    }
}

val person = Person("สมเกียรติ", 25)
person.introduce()

// Data Class
data class User(
    val id: Int,
    val username: String,
    val email: String
)

val user = User(1, "somkiat", "somkiat@example.com")
val user2 = user.copy(username = "somkiat2")

// Class with Properties
class Student(val name: String) {
    var grade: String = "A"
        get() = field
        set(value) {
            field = if (value.length == 1) value else "A"
        }
    
    val isPassed: Boolean
        get() = grade != "F"
}

// Inheritance
open class Animal(val name: String) {
    open fun makeSound() {
        println("Animal sound")
    }
}

class Dog(name: String) : Animal(name) {
    override fun makeSound() {
        println("Woof!")
    }
}

// Object (Singleton)
object DatabaseConfig {
    const val URL = "localhost:3306"
    const val USERNAME = "admin"
    
    fun connect() {
        println("Connecting to $URL")
    }
}

// Companion Object
class MathUtils {
    companion object {
        const val PI = 3.14159
        
        fun square(x: Int) = x * x
    }
}

val squared = MathUtils.square(5)
```

### 8. Null Safety (15 นาที)

```kotlin
// Nullable Types
var text: String? = null

// Safe Call Operator
val length = text?.length  // null if text is null

// Elvis Operator
val len = text?.length ?: 0  // 0 if text is null

// Not-null Assertion
val len2 = text!!.length  // Throws exception if null

// Safe Cast
val number: Int? = text as? Int  // null if cannot cast

// Let Function
text?.let {
    println("Text is: $it")
}
```

## 💻 แบบฝึกหัด

### Exercise 1: คำนวณ BMI
สร้าง function คำนวณ BMI และบอกผลว่าอยู่ในเกณฑ์ไหน

```kotlin
fun calculateBMI(weight: Double, height: Double): String {
    // TODO: implement this
}

// เกณฑ์:
// < 18.5 = ผอม
// 18.5-24.9 = ปกติ
// 25-29.9 = อ้วน
// >= 30 = อ้วนมาก
```

### Exercise 2: Student Management
สร้าง data class Student และ functions เพื่อจัดการข้อมูลนักเรียน

```kotlin
data class Student(
    val id: Int,
    val name: String,
    var score: Int
)

// TODO: Create functions
// 1. addStudent
// 2. removeStudent
// 3. getTopStudents (top 3)
// 4. getAverageScore
```

### Exercise 3: Shopping Cart
สร้างระบบตะกร้าสินค้าง่ายๆ

```kotlin
data class Product(
    val id: Int,
    val name: String,
    val price: Double
)

// TODO: Create ShoppingCart class with:
// - addProduct
// - removeProduct
// - getTotalPrice
// - applyDiscount (percentage)
```

## 📝 เฉลย

เฉลยแบบฝึกหัดอยู่ที่: [solutions/session1-solutions.kt](../../solutions/session1-solutions.kt)

## 🔗 Resources

- [Kotlin Official Documentation](https://kotlinlang.org/docs/home.html)
- [Kotlin Playground](https://play.kotlinlang.org/)
- [Kotlin Koans](https://kotlinlang.org/docs/koans.html)

## ⏭️ Next Session

[Session 2: Android Basics](../session2-android-basics/README.md)
