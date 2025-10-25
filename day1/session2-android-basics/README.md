# Session 2: Android Basics (3 ชั่วโมง)

## 🎯 วัตถุประสงค์
เรียนรู้โครงสร้างของ Android App และสร้างแอพแรกของคุณ

## 📚 เนื้อหา

### 1. Android Studio Setup (30 นาที)

#### ติดตั้ง Android Studio
1. Download จาก https://developer.android.com/studio
2. ติดตั้งและเปิดครั้งแรก
3. ติดตั้ง SDK และ Tools ที่จำเป็น
4. สร้าง Virtual Device (Emulator)

#### Components ที่สำคัญ:
- SDK Manager
- AVD Manager (Android Virtual Device)
- Logcat
- Device File Explorer

### 2. สร้างโปรเจคแรก (45 นาที)

#### ขั้นตอนสร้าง Project:
1. **New Project** → **Empty Activity**
2. ตั้งค่า:
   - Name: `HelloKotlin`
   - Package name: `com.example.hellokotlin`
   - Language: **Kotlin**
   - Minimum SDK: **API 24 (Android 7.0)**

#### โครงสร้างโปรเจค:

```
HelloKotlin/
├── app/
│   ├── src/
│   │   ├── main/
│   │   │   ├── java/com/example/hellokotlin/
│   │   │   │   └── MainActivity.kt
│   │   │   ├── res/
│   │   │   │   ├── layout/
│   │   │   │   │   └── activity_main.xml
│   │   │   │   ├── values/
│   │   │   │   │   ├── strings.xml
│   │   │   │   │   ├── colors.xml
│   │   │   │   │   └── themes.xml
│   │   │   │   ├── drawable/
│   │   │   │   └── mipmap/
│   │   │   └── AndroidManifest.xml
│   │   └── androidTest/
│   │   └── test/
│   └── build.gradle.kts
├── gradle/
└── build.gradle.kts
```

### 3. Android Manifest (20 นาที)

**AndroidManifest.xml** - ไฟล์กำหนดค่าหลักของแอพ

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.hellokotlin">

    <!-- Permissions -->
    <uses-permission android:name="android.permission.INTERNET" />

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.HelloKotlin">
        
        <!-- Main Activity -->
        <activity
            android:name=".MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>
</manifest>
```

### 4. Activity Lifecycle (45 นาที)

#### Activity คืออะไร?
Activity = หน้าจอหนึ่งของแอพ

#### Lifecycle Methods:

```kotlin
class MainActivity : AppCompatActivity() {
    
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        
        Log.d("Lifecycle", "onCreate called")
        // เรียกครั้งแรกเมื่อสร้าง Activity
        // ใช้สำหรับ initialize components
    }
    
    override fun onStart() {
        super.onStart()
        Log.d("Lifecycle", "onStart called")
        // Activity กำลังจะแสดงบนหน้าจอ
    }
    
    override fun onResume() {
        super.onResume()
        Log.d("Lifecycle", "onResume called")
        // Activity แสดงบนหน้าจอและพร้อมใช้งาน
        // เริ่ม animations, sensors
    }
    
    override fun onPause() {
        super.onPause()
        Log.d("Lifecycle", "onPause called")
        // Activity กำลังจะถูกซ่อน
        // หยุด animations, sensors
        // บันทึกข้อมูล
    }
    
    override fun onStop() {
        super.onStop()
        Log.d("Lifecycle", "onStop called")
        // Activity ถูกซ่อนแล้ว
        // ปล่อย resources
    }
    
    override fun onDestroy() {
        super.onDestroy()
        Log.d("Lifecycle", "onDestroy called")
        // Activity กำลังจะถูกทำลาย
        // ทำความสะอาดสุดท้าย
    }
    
    override fun onRestart() {
        super.onRestart()
        Log.d("Lifecycle", "onRestart called")
        // เรียกเมื่อ Activity กลับมาจาก stopped state
    }
}
```

#### Lifecycle Diagram:
```
        onCreate()
            ↓
        onStart()
            ↓
        onResume() ←---┐
            ↓          |
    [App Running]      |
            ↓          |
        onPause() -----┘
            ↓
        onStop()
            ↓
        onDestroy()
```

### 5. Resources (30 นาที)

#### strings.xml
```xml
<resources>
    <string name="app_name">Hello Kotlin</string>
    <string name="welcome_message">ยินดีต้อนรับสู่ Kotlin Android</string>
    <string name="button_click">คลิกฉัน</string>
</resources>
```

#### colors.xml
```xml
<resources>
    <color name="purple_200">#FFBB86FC</color>
    <color name="purple_500">#FF6200EE</color>
    <color name="purple_700">#FF3700B3</color>
    <color name="teal_200">#FF03DAC5</color>
    <color name="teal_700">#FF018786</color>
    <color name="black">#FF000000</color>
    <color name="white">#FFFFFFFF</color>
</resources>
```

#### ใช้ Resources ใน Code:
```kotlin
// Get String
val appName = getString(R.string.app_name)

// Get Color
val color = ContextCompat.getColor(this, R.color.purple_500)

// Get Drawable
val drawable = ContextCompat.getDrawable(this, R.drawable.ic_launcher)
```

### 6. Gradle (30 นาที)

#### build.gradle.kts (Project Level)
```kotlin
plugins {
    id("com.android.application") version "8.1.0" apply false
    id("org.jetbrains.kotlin.android") version "1.9.0" apply false
}
```

#### build.gradle.kts (Module Level)
```kotlin
plugins {
    id("com.android.application")
    id("org.jetbrains.kotlin.android")
}

android {
    namespace = "com.example.hellokotlin"
    compileSdk = 34

    defaultConfig {
        applicationId = "com.example.hellokotlin"
        minSdk = 24
        targetSdk = 34
        versionCode = 1
        versionName = "1.0"
    }

    buildTypes {
        release {
            isMinifyEnabled = false
            proguardFiles(
                getDefaultProguardFile("proguard-android-optimize.txt"),
                "proguard-rules.pro"
            )
        }
    }
    
    compileOptions {
        sourceCompatibility = JavaVersion.VERSION_1_8
        targetCompatibility = JavaVersion.VERSION_1_8
    }
    
    kotlinOptions {
        jvmTarget = "1.8"
    }
}

dependencies {
    implementation("androidx.core:core-ktx:1.12.0")
    implementation("androidx.appcompat:appcompat:1.6.1")
    implementation("com.google.android.material:material:1.10.0")
    implementation("androidx.constraintlayout:constraintlayout:2.1.4")
}
```

### 7. สร้างแอพ Hello World (30 นาที)

#### activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <TextView
        android:id="@+id/textView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/welcome_message"
        android:textSize="24sp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <Button
        android:id="@+id/buttonClick"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/button_click"
        android:layout_marginTop="32dp"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@id/textView" />

</androidx.constraintlayout.widget.ConstraintLayout>
```

#### MainActivity.kt
```kotlin
package com.example.hellokotlin

import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import android.widget.Button
import android.widget.TextView
import android.widget.Toast

class MainActivity : AppCompatActivity() {
    
    private lateinit var textView: TextView
    private lateinit var button: Button
    private var clickCount = 0
    
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        
        // Initialize views
        textView = findViewById(R.id.textView)
        button = findViewById(R.id.buttonClick)
        
        // Set click listener
        button.setOnClickListener {
            clickCount++
            textView.text = "คุณคลิก $clickCount ครั้ง"
            
            Toast.makeText(
                this,
                "คลิกแล้ว!",
                Toast.LENGTH_SHORT
            ).show()
        }
    }
}
```

## 💻 แบบฝึกหัด

### Exercise 1: Counter App
สร้างแอพนับเลขที่มี:
- ปุ่ม + เพิ่มเลข
- ปุ่ม - ลดเลข
- ปุ่ม Reset
- แสดงตัวเลขตรงกลาง

### Exercise 2: Lifecycle Logger
สร้างแอพที่แสดง log ทุก lifecycle method พร้อม timestamp

### Exercise 3: Color Changer
สร้างแอพที่:
- มีปุ่มหลายสี
- กดปุ่มแล้วพื้นหลังเปลี่ยนสี
- แสดงชื่อสีที่เลือก

## 📝 Tips

- ใช้ `Log.d()` สำหรับ debug
- ใช้ `Toast` สำหรับแสดงข้อความสั้นๆ
- ใช้ `findViewById()` หรือ View Binding ในการเข้าถึง views
- อ่าน Logcat เมื่อเกิด error

## 🔗 Resources

- [Android Developer Guide](https://developer.android.com/guide)
- [Activity Lifecycle](https://developer.android.com/guide/components/activities/activity-lifecycle)
- [Gradle User Guide](https://docs.gradle.org/current/userguide/userguide.html)

## ⏭️ Next Session

[Session 3: UI Components](../session3-ui-components/README.md)
