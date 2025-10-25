# Session 4: Hands-on Practice (2 ชั่วโมง)

## 🎯 วัตถุประสงค์
ฝึกปฏิบัติสร้างแอพจริงด้วยความรู้ที่ได้เรียนมา

## 📚 โปรเจค: Calculator App (เครื่องคิดเลข)

สร้างแอพเครื่องคิดเลขพื้นฐานที่สามารถ บวก ลบ คูณ หาร

### ฟีเจอร์:
- แสดงตัวเลขและผลลัพธ์
- ปุ่มตัวเลข 0-9
- ปุ่มดำเนินการ +, -, ×, ÷
- ปุ่ม = สำหรับคำนวณ
- ปุ่ม C สำหรับเคลียร์
- ปุ่มจุดทศนิยม

### UI Design

```
┌────────────────────────┐
│   123456789           │  ← Display
├────────────────────────┤
│  7  │  8  │  9  │  ÷  │
├─────┼─────┼─────┼─────┤
│  4  │  5  │  6  │  ×  │
├─────┼─────┼─────┼─────┤
│  1  │  2  │  3  │  -  │
├─────┼─────┼─────┼─────┤
│  C  │  0  │  .  │  +  │
├────────────────────────┤
│         =              │
└────────────────────────┘
```

## 📝 Step-by-Step Implementation

### Step 1: สร้าง UI Layout

**activity_calculator.xml**
```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:padding="16dp"
    android:background="#F5F5F5">

    <!-- Display -->
    <TextView
        android:id="@+id/tvDisplay"
        android:layout_width="0dp"
        android:layout_height="100dp"
        android:text="0"
        android:textSize="48sp"
        android:textColor="#000000"
        android:gravity="end|center_vertical"
        android:padding="16dp"
        android:background="#FFFFFF"
        android:elevation="4dp"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent" />

    <!-- Number Pad -->
    <GridLayout
        android:id="@+id/gridLayout"
        android:layout_width="0dp"
        android:layout_height="0dp"
        android:columnCount="4"
        android:rowCount="5"
        android:layout_marginTop="16dp"
        app:layout_constraintTop_toBottomOf="@id/tvDisplay"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent">

        <!-- Row 1: 7, 8, 9, ÷ -->
        <Button android:id="@+id/btn7" style="@style/NumberButton" android:text="7" />
        <Button android:id="@+id/btn8" style="@style/NumberButton" android:text="8" />
        <Button android:id="@+id/btn9" style="@style/NumberButton" android:text="9" />
        <Button android:id="@+id/btnDivide" style="@style/OperatorButton" android:text="÷" />

        <!-- Row 2: 4, 5, 6, × -->
        <Button android:id="@+id/btn4" style="@style/NumberButton" android:text="4" />
        <Button android:id="@+id/btn5" style="@style/NumberButton" android:text="5" />
        <Button android:id="@+id/btn6" style="@style/NumberButton" android:text="6" />
        <Button android:id="@+id/btnMultiply" style="@style/OperatorButton" android:text="×" />

        <!-- Row 3: 1, 2, 3, - -->
        <Button android:id="@+id/btn1" style="@style/NumberButton" android:text="1" />
        <Button android:id="@+id/btn2" style="@style/NumberButton" android:text="2" />
        <Button android:id="@+id/btn3" style="@style/NumberButton" android:text="3" />
        <Button android:id="@+id/btnSubtract" style="@style/OperatorButton" android:text="-" />

        <!-- Row 4: C, 0, ., + -->
        <Button android:id="@+id/btnClear" style="@style/ClearButton" android:text="C" />
        <Button android:id="@+id/btn0" style="@style/NumberButton" android:text="0" />
        <Button android:id="@+id/btnDot" style="@style/NumberButton" android:text="." />
        <Button android:id="@+id/btnAdd" style="@style/OperatorButton" android:text="+" />

        <!-- Row 5: = (spanning 4 columns) -->
        <Button 
            android:id="@+id/btnEquals"
            style="@style/EqualsButton"
            android:text="="
            android:layout_columnSpan="4" />
    </GridLayout>

</androidx.constraintlayout.widget.ConstraintLayout>
```

### Step 2: สร้าง Styles

**res/values/styles.xml**
```xml
<resources>
    <style name="NumberButton">
        <item name="android:layout_width">0dp</item>
        <item name="android:layout_height">0dp</item>
        <item name="android:layout_columnWeight">1</item>
        <item name="android:layout_rowWeight">1</item>
        <item name="android:layout_margin">4dp</item>
        <item name="android:textSize">24sp</item>
        <item name="android:background">@drawable/button_number</item>
    </style>

    <style name="OperatorButton" parent="NumberButton">
        <item name="android:background">@drawable/button_operator</item>
        <item name="android:textColor">@color/white</item>
    </style>

    <style name="ClearButton" parent="NumberButton">
        <item name="android:background">@drawable/button_clear</item>
        <item name="android:textColor">@color/white</item>
    </style>

    <style name="EqualsButton" parent="NumberButton">
        <item name="android:background">@drawable/button_equals</item>
        <item name="android:textColor">@color/white</item>
    </style>
</resources>
```

### Step 3: สร้าง Button Drawables

**res/drawable/button_number.xml**
```xml
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android">
    <solid android:color="#FFFFFF" />
    <corners android:radius="8dp" />
    <stroke android:width="1dp" android:color="#DDDDDD" />
</shape>
```

**res/drawable/button_operator.xml**
```xml
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android">
    <solid android:color="#FF9800" />
    <corners android:radius="8dp" />
</shape>
```

**res/drawable/button_clear.xml**
```xml
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android">
    <solid android:color="#F44336" />
    <corners android:radius="8dp" />
</shape>
```

**res/drawable/button_equals.xml**
```xml
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android">
    <solid android:color="#4CAF50" />
    <corners android:radius="8dp" />
</shape>
```

### Step 4: Implement Calculator Logic

**CalculatorActivity.kt**
```kotlin
package com.example.calculator

import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import android.widget.Button
import android.widget.TextView
import android.widget.Toast

class CalculatorActivity : AppCompatActivity() {

    private lateinit var tvDisplay: TextView
    private var currentNumber = ""
    private var previousNumber = ""
    private var operation = ""
    private var isNewOperation = true

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_calculator)

        tvDisplay = findViewById(R.id.tvDisplay)

        // Number buttons
        val numberButtons = listOf(
            R.id.btn0, R.id.btn1, R.id.btn2, R.id.btn3, R.id.btn4,
            R.id.btn5, R.id.btn6, R.id.btn7, R.id.btn8, R.id.btn9
        )

        numberButtons.forEach { id ->
            findViewById<Button>(id).setOnClickListener { view ->
                onNumberClick((view as Button).text.toString())
            }
        }

        // Operator buttons
        findViewById<Button>(R.id.btnAdd).setOnClickListener { onOperatorClick("+") }
        findViewById<Button>(R.id.btnSubtract).setOnClickListener { onOperatorClick("-") }
        findViewById<Button>(R.id.btnMultiply).setOnClickListener { onOperatorClick("×") }
        findViewById<Button>(R.id.btnDivide).setOnClickListener { onOperatorClick("÷") }

        // Other buttons
        findViewById<Button>(R.id.btnDot).setOnClickListener { onDotClick() }
        findViewById<Button>(R.id.btnEquals).setOnClickListener { onEqualsClick() }
        findViewById<Button>(R.id.btnClear).setOnClickListener { onClearClick() }
    }

    private fun onNumberClick(number: String) {
        if (isNewOperation) {
            currentNumber = number
            isNewOperation = false
        } else {
            currentNumber += number
        }
        updateDisplay()
    }

    private fun onOperatorClick(op: String) {
        if (currentNumber.isNotEmpty()) {
            if (previousNumber.isNotEmpty() && !isNewOperation) {
                calculate()
            }
            previousNumber = currentNumber
            operation = op
            isNewOperation = true
        }
    }

    private fun onDotClick() {
        if (isNewOperation) {
            currentNumber = "0."
            isNewOperation = false
        } else if (!currentNumber.contains(".")) {
            currentNumber += "."
        }
        updateDisplay()
    }

    private fun onEqualsClick() {
        if (previousNumber.isNotEmpty() && currentNumber.isNotEmpty()) {
            calculate()
            operation = ""
            previousNumber = ""
            isNewOperation = true
        }
    }

    private fun calculate() {
        try {
            val num1 = previousNumber.toDouble()
            val num2 = currentNumber.toDouble()
            val result = when (operation) {
                "+" -> num1 + num2
                "-" -> num1 - num2
                "×" -> num1 * num2
                "÷" -> {
                    if (num2 != 0.0) {
                        num1 / num2
                    } else {
                        Toast.makeText(this, "ไม่สามารถหารด้วย 0 ได้", Toast.LENGTH_SHORT).show()
                        return
                    }
                }
                else -> return
            }

            currentNumber = if (result % 1.0 == 0.0) {
                result.toInt().toString()
            } else {
                String.format("%.2f", result)
            }
            updateDisplay()
        } catch (e: Exception) {
            Toast.makeText(this, "เกิดข้อผิดพลาด", Toast.LENGTH_SHORT).show()
            onClearClick()
        }
    }

    private fun onClearClick() {
        currentNumber = ""
        previousNumber = ""
        operation = ""
        isNewOperation = true
        tvDisplay.text = "0"
    }

    private fun updateDisplay() {
        tvDisplay.text = if (currentNumber.isEmpty()) "0" else currentNumber
    }
}
```

## 💻 แบบฝึกหัดเพิ่มเติม

### Challenge 1: เพิ่มฟีเจอร์
- ปุ่ม backspace (ลบตัวเลขทีละตัว)
- แสดงการดำเนินการที่กำลังทำ
- History ของการคำนวณ
- รองรับเปอร์เซ็นต์ (%)

### Challenge 2: ปรับปรุง UI
- เพิ่ม animation เมื่อกดปุ่ม
- เปลี่ยนสีปุ่มเมื่อกด
- แสดง result แบบ real-time
- รองรับ landscape mode

### Challenge 3: Advanced Features
- บันทึก history ด้วย SharedPreferences
- เพิ่มฟังก์ชันทางวิทยาศาสตร์ (sin, cos, sqrt, etc.)
- รองรับ parentheses ( )

## 📝 Testing Checklist

- [ ] กดตัวเลขแสดงผลถูกต้อง
- [ ] การบวก ลบ คูณ หาร ทำงานถูกต้อง
- [ ] กด C แล้วเคลียร์หมด
- [ ] จุดทศนิยมใช้ได้ปกติ
- [ ] หารด้วย 0 แสดง error
- [ ] หน้าจอหมุนยังใช้งานได้
- [ ] ตัวเลขยาวไม่ล้นหน้าจอ

## 🔗 Resources

- [Calculator Tutorial](https://developer.android.com/codelabs/basic-android-kotlin-compose-first-app)
- [GridLayout Guide](https://developer.android.com/reference/android/widget/GridLayout)

## 🎉 Summary Day 1

วันนี้คุณได้เรียนรู้:
- ✅ พื้นฐาน Kotlin
- ✅ Android Activity Lifecycle
- ✅ UI Components และ Layouts
- ✅ Event Handling
- ✅ สร้างแอพจริง (Calculator)

## ⏭️ Day 2

[Day 2: Advanced Topics](../../day2/session5-data-storage/README.md)

พรุ่งนี้เราจะเรียนรู้:
- Data Storage (SharedPreferences, Room Database)
- Networking & APIs
- สร้างแอพสมบูรณ์
- เผยแพร่แอพ
