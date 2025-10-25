# Session 3: UI Components (2 ชั่วโมง)

## 🎯 วัตถุประสงค์
เรียนรู้การออกแบบ UI ด้วย XML และ Views ต่างๆ

## 📚 เนื้อหา

### 1. Views และ ViewGroups (20 นาที)

#### View
- คือ UI component พื้นฐาน (Button, TextView, EditText, etc.)
- แต่ละ View มี attributes เช่น width, height, padding, margin

#### ViewGroup
- คือ container สำหรับ Views อื่นๆ
- ใช้จัดการ layout (LinearLayout, ConstraintLayout, etc.)

### 2. Common Layouts (40 นาที)

#### LinearLayout
จัด Views เป็นแถวหรือคอลัมน์

```xml
<LinearLayout
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:orientation="vertical"
    android:padding="16dp">
    
    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="ชื่อ" />
    
    <EditText
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="กรอกชื่อของคุณ" />
    
    <Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="ส่ง" />
</LinearLayout>
```

#### ConstraintLayout
จัด Views โดยกำหนด constraints (ข้อจำกัด)

```xml
<androidx.constraintlayout.widget.ConstraintLayout
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    
    <TextView
        android:id="@+id/title"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="ยินดีต้อนรับ"
        android:textSize="24sp"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        android:layout_marginTop="32dp" />
    
    <EditText
        android:id="@+id/editText"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:hint="ข้อความ"
        app:layout_constraintTop_toBottomOf="@id/title"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        android:layout_margin="16dp" />
    
    <Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="คลิก"
        app:layout_constraintTop_toBottomOf="@id/editText"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        android:layout_marginTop="16dp" />
        
</androidx.constraintlayout.widget.ConstraintLayout>
```

#### RelativeLayout
จัด Views โดยอ้างอิงตำแหน่งซึ่งกันและกัน

```xml
<RelativeLayout
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:padding="16dp">
    
    <TextView
        android:id="@+id/header"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="หัวข้อ"
        android:layout_centerHorizontal="true" />
    
    <Button
        android:id="@+id/btnLeft"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="ซ้าย"
        android:layout_below="@id/header"
        android:layout_alignParentStart="true"
        android:layout_marginTop="16dp" />
    
    <Button
        android:id="@+id/btnRight"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="ขวา"
        android:layout_below="@id/header"
        android:layout_alignParentEnd="true"
        android:layout_marginTop="16dp" />
        
</RelativeLayout>
```

### 3. Common Widgets (40 นาที)

#### TextView
แสดงข้อความ

```xml
<TextView
    android:id="@+id/textView"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="สวัสดี Kotlin"
    android:textSize="18sp"
    android:textColor="#000000"
    android:textStyle="bold"
    android:gravity="center"
    android:padding="16dp" />
```

```kotlin
val textView = findViewById<TextView>(R.id.textView)
textView.text = "ข้อความใหม่"
textView.setTextColor(Color.RED)
```

#### Button
ปุ่มกด

```xml
<Button
    android:id="@+id/button"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="คลิกที่นี่"
    android:background="@color/purple_500"
    android:textColor="@color/white"
    android:padding="12dp" />
```

```kotlin
val button = findViewById<Button>(R.id.button)
button.setOnClickListener {
    // Do something
    Toast.makeText(this, "คลิกแล้ว!", Toast.LENGTH_SHORT).show()
}
```

#### EditText
ช่องกรอกข้อมูล

```xml
<EditText
    android:id="@+id/editText"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:hint="กรอกข้อความ"
    android:inputType="text"
    android:maxLines="1"
    android:padding="12dp" />

<!-- Input Types -->
<!-- textEmailAddress - อีเมล -->
<!-- phone - เบอร์โทร -->
<!-- number - ตัวเลข -->
<!-- textPassword - รหัสผ่าน -->
```

```kotlin
val editText = findViewById<EditText>(R.id.editText)

// Get text
val text = editText.text.toString()

// Set text
editText.setText("ข้อความใหม่")

// Clear text
editText.text.clear()

// Listen to text changes
editText.addTextChangedListener(object : TextWatcher {
    override fun beforeTextChanged(s: CharSequence?, start: Int, count: Int, after: Int) {}
    
    override fun onTextChanged(s: CharSequence?, start: Int, before: Int, count: Int) {
        // Text is changing
    }
    
    override fun afterTextChanged(s: Editable?) {
        // Text changed
    }
})
```

#### ImageView
แสดงรูปภาพ

```xml
<ImageView
    android:id="@+id/imageView"
    android:layout_width="200dp"
    android:layout_height="200dp"
    android:src="@drawable/ic_launcher"
    android:scaleType="centerCrop"
    android:contentDescription="รูปภาพ" />

<!-- Scale Types -->
<!-- centerCrop - ครอบและเซ็นเตอร์ -->
<!-- fitCenter - พอดีและเซ็นเตอร์ -->
<!-- fitXY - ยืดเต็ม -->
```

```kotlin
val imageView = findViewById<ImageView>(R.id.imageView)

// Set image from drawable
imageView.setImageResource(R.drawable.ic_launcher)

// Set image from bitmap
val bitmap = BitmapFactory.decodeResource(resources, R.drawable.ic_launcher)
imageView.setImageBitmap(bitmap)
```

#### CheckBox
ช่องติ๊ก

```xml
<CheckBox
    android:id="@+id/checkbox"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="ยอมรับเงื่อนไข" />
```

```kotlin
val checkbox = findViewById<CheckBox>(R.id.checkbox)

checkbox.setOnCheckedChangeListener { buttonView, isChecked ->
    if (isChecked) {
        Toast.makeText(this, "ติ๊กแล้ว", Toast.LENGTH_SHORT).show()
    }
}

// Check programmatically
checkbox.isChecked = true
```

#### RadioButton
ปุ่มเลือกหนึ่งตัวเลือก

```xml
<RadioGroup
    android:id="@+id/radioGroup"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content">
    
    <RadioButton
        android:id="@+id/radioMale"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="ชาย" />
    
    <RadioButton
        android:id="@+id/radioFemale"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="หญิง" />
        
</RadioGroup>
```

```kotlin
val radioGroup = findViewById<RadioGroup>(R.id.radioGroup)

radioGroup.setOnCheckedChangeListener { group, checkedId ->
    when (checkedId) {
        R.id.radioMale -> Toast.makeText(this, "เลือกชาย", Toast.LENGTH_SHORT).show()
        R.id.radioFemale -> Toast.makeText(this, "เลือกหญิง", Toast.LENGTH_SHORT).show()
    }
}
```

### 4. ตกแต่ง UI (20 นาที)

#### Styles
ใน res/values/styles.xml

```xml
<resources>
    <style name="CustomButton">
        <item name="android:textColor">@color/white</item>
        <item name="android:background">@color/purple_500</item>
        <item name="android:padding">12dp</item>
    </style>
</resources>
```

ใช้งาน:
```xml
<Button
    style="@style/CustomButton"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="ปุ่ม" />
```

#### Drawables
สร้างปุ่มกลมที่สวยงาม (res/drawable/rounded_button.xml)

```xml
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android"
    android:shape="rectangle">
    <solid android:color="@color/purple_500" />
    <corners android:radius="24dp" />
    <padding
        android:left="16dp"
        android:right="16dp"
        android:top="12dp"
        android:bottom="12dp" />
</shape>
```

ใช้งาน:
```xml
<Button
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="ปุ่มกลม"
    android:background="@drawable/rounded_button" />
```

## 💻 แบบฝึกหัด: Login Screen

สร้างหน้า Login ที่สวยงามด้วย:
- Logo ด้านบน (ImageView)
- ช่องกรอก Email (EditText with email input type)
- ช่องกรอก Password (EditText with password input type)
- CheckBox "จำฉันไว้"
- ปุ่ม Login
- ข้อความ "ยังไม่มีบัญชี? สมัครเลย"

### activity_login.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:padding="24dp">

    <!-- TODO: เพิ่ม UI components ตามที่กำหนด -->

</androidx.constraintlayout.widget.ConstraintLayout>
```

### LoginActivity.kt
```kotlin
class LoginActivity : AppCompatActivity() {
    
    private lateinit var editEmail: EditText
    private lateinit var editPassword: EditText
    private lateinit var checkboxRemember: CheckBox
    private lateinit var buttonLogin: Button
    
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_login)
        
        // TODO: Initialize views and set listeners
    }
    
    private fun validateAndLogin() {
        val email = editEmail.text.toString()
        val password = editPassword.text.toString()
        
        // TODO: Validate and show appropriate messages
    }
}
```

## 📝 เฉลย

เฉลยแบบฝึกหัดอยู่ที่: [solutions/session3-solutions/](../../solutions/session3-solutions/)

## 🔗 Resources

- [Layouts Guide](https://developer.android.com/guide/topics/ui/declaring-layout)
- [Material Design Components](https://material.io/components)
- [View Reference](https://developer.android.com/reference/android/view/View)

## ⏭️ Next Session

[Session 4: Hands-on Practice](../session4-practice/README.md)
