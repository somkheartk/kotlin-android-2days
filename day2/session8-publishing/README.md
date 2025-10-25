# Session 8: Publishing & Best Practices (1 ชั่วโมง)

## 🎯 วัตถุประสงค์
เรียนรู้การเตรียมและเผยแพร่แอพบน Google Play Store

## 📚 เนื้อหา

### 1. App Signing (15 นาที)

#### สร้าง Keystore

```bash
keytool -genkey -v -keystore my-release-key.jks \
  -keyalg RSA -keysize 2048 -validity 10000 \
  -alias my-key-alias
```

#### กำหนดค่าใน build.gradle.kts

```kotlin
android {
    signingConfigs {
        create("release") {
            storeFile = file("my-release-key.jks")
            storePassword = "your-password"
            keyAlias = "my-key-alias"
            keyPassword = "your-password"
        }
    }

    buildTypes {
        release {
            signingConfig = signingConfigs.getByName("release")
            isMinifyEnabled = true
            isShrinkResources = true
            proguardFiles(
                getDefaultProguardFile("proguard-android-optimize.txt"),
                "proguard-rules.pro"
            )
        }
    }
}
```

**หมายเหตุ**: ไม่ควรเก็บ password ใน code จริง ควรใช้:
- `keystore.properties` file
- Environment variables
- Gradle properties

#### keystore.properties
```properties
storePassword=myStorePassword
keyPassword=myKeyPassword
keyAlias=myKeyAlias
storeFile=my-release-key.jks
```

#### อ่านจาก keystore.properties
```kotlin
val keystorePropertiesFile = rootProject.file("keystore.properties")
val keystoreProperties = Properties()
keystoreProperties.load(FileInputStream(keystorePropertiesFile))

android {
    signingConfigs {
        create("release") {
            keyAlias = keystoreProperties["keyAlias"] as String
            keyPassword = keystoreProperties["keyPassword"] as String
            storeFile = file(keystoreProperties["storeFile"] as String)
            storePassword = keystoreProperties["storePassword"] as String
        }
    }
}
```

### 2. Build APK/AAB (10 นาที)

#### Build APK
```bash
./gradlew assembleRelease
```
ไฟล์จะอยู่ที่: `app/build/outputs/apk/release/app-release.apk`

#### Build AAB (Android App Bundle) - แนะนำสำหรับ Play Store
```bash
./gradlew bundleRelease
```
ไฟล์จะอยู่ที่: `app/build/outputs/bundle/release/app-release.aab`

**ทำไมต้องใช้ AAB?**
- ขนาดเล็กกว่า APK
- Google Play จะสร้าง APK ที่เหมาะสมสำหรับแต่ละอุปกรณ์
- รองรับ Dynamic Features

### 3. ProGuard & R8 (10 นาที)

ProGuard/R8 ช่วย:
- ลดขนาดแอพ (Code shrinking)
- Obfuscate code (ป้องกันการ reverse engineer)
- Optimize code

**proguard-rules.pro**
```proguard
# Keep data classes
-keep class com.example.taskmaster.data.** { *; }

# Keep Retrofit
-keepattributes Signature
-keepattributes *Annotation*
-keep class retrofit2.** { *; }

# Keep Room
-keep class * extends androidx.room.RoomDatabase
-keep @androidx.room.Entity class *
-dontwarn androidx.room.paging.**

# Keep Gson
-keep class com.google.gson.** { *; }

# Keep custom classes
-keep class com.example.taskmaster.domain.model.** { *; }
```

### 4. App Icons & Assets (5 นาที)

#### Icon Sizes
- `mipmap-mdpi`: 48x48
- `mipmap-hdpi`: 72x72
- `mipmap-xhdpi`: 96x96
- `mipmap-xxhdpi`: 144x144
- `mipmap-xxxhdpi`: 192x192

**เครื่องมือสร้าง Icon**:
- Android Studio Image Asset Studio
- [Android Asset Studio](http://romannurik.github.io/AndroidAssetStudio/)
- [App Icon Generator](https://appicon.co/)

### 5. Versioning (5 นาที)

**build.gradle.kts**
```kotlin
android {
    defaultConfig {
        versionCode = 1      // เพิ่มทุกครั้งที่ release
        versionName = "1.0.0" // แสดงให้ user เห็น
    }
}
```

**Version Name Convention (Semantic Versioning)**:
- `MAJOR.MINOR.PATCH`
- `1.0.0` → First release
- `1.0.1` → Bug fix
- `1.1.0` → New features
- `2.0.0` → Breaking changes

### 6. Google Play Console (15 นาที)

#### ขั้นตอนการเผยแพร่:

1. **สร้าง Developer Account**
   - ไปที่ https://play.google.com/console
   - ชำระค่าสมัคร $25 (ครั้งเดียว)

2. **สร้าง App ใหม่**
   - เลือก "Create app"
   - กรอกข้อมูลพื้นฐาน

3. **App Content**
   - Privacy Policy (จำเป็น)
   - Target audience
   - Content rating
   - App category

4. **Store Listing**
   - App name
   - Short description (80 characters)
   - Full description (4000 characters)
   - Screenshots (2-8 images)
   - Feature graphic (1024x500)
   - App icon (512x512)

5. **Pricing & Distribution**
   - Free หรือ Paid
   - เลือกประเทศที่จะเผยแพร่

6. **Release**
   - Upload AAB file
   - Release notes
   - Submit for review

#### Screenshots Requirements:
- ขั้นต่ำ 2 screenshots
- JPEG หรือ 24-bit PNG
- ขนาด 320px - 3840px
- Aspect ratio 16:9 หรือ 9:16

### 7. Best Practices (15 นาที)

#### Code Quality
```kotlin
// ✅ Good
fun calculateTotal(items: List<Item>): Double {
    return items.sumOf { it.price }
}

// ❌ Bad
fun calc(i: List<Item>): Double {
    var t = 0.0
    for (x in i) t += x.price
    return t
}
```

#### Resource Naming
```xml
<!-- ✅ Good -->
<string name="login_button_text">เข้าสู่ระบบ</string>
<color name="primary_blue">#2196F3</color>
<dimen name="spacing_small">8dp</dimen>

<!-- ❌ Bad -->
<string name="text1">เข้าสู่ระบบ</string>
<color name="blue">#2196F3</color>
<dimen name="sp">8dp</dimen>
```

#### Error Handling
```kotlin
// ✅ Good
try {
    val result = apiService.getData()
    handleSuccess(result)
} catch (e: IOException) {
    handleNetworkError()
} catch (e: HttpException) {
    handleApiError(e.code())
} catch (e: Exception) {
    handleUnknownError()
}

// ❌ Bad
try {
    val result = apiService.getData()
} catch (e: Exception) {
    // Do nothing
}
```

#### Memory Leaks
```kotlin
// ✅ Good - Use lifecycle-aware components
class MyActivity : AppCompatActivity() {
    private lateinit var binding: ActivityMainBinding
    
    override fun onDestroy() {
        super.onDestroy()
        // Clean up
    }
}

// ❌ Bad - Static reference to Activity
companion object {
    var activity: Activity? = null
}
```

#### Performance
```kotlin
// ✅ Good - Use ViewBinding
class MyActivity : AppCompatActivity() {
    private lateinit var binding: ActivityMainBinding
    
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        binding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(binding.root)
        
        binding.textView.text = "Hello"
    }
}

// ❌ Bad - Multiple findViewById calls
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_main)
    
    findViewById<TextView>(R.id.textView).text = "Hello"
}
```

#### Security
```kotlin
// ✅ Good - Don't hardcode sensitive data
object ApiConfig {
    val BASE_URL = BuildConfig.API_URL
    val API_KEY = BuildConfig.API_KEY
}

// ❌ Bad
const val API_KEY = "my-secret-key-12345"
```

#### Accessibility
```xml
<!-- ✅ Good -->
<ImageButton
    android:id="@+id/btnSave"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:src="@drawable/ic_save"
    android:contentDescription="@string/save_button_description" />

<!-- ❌ Bad -->
<ImageButton
    android:src="@drawable/ic_save" />
```

### 8. Testing Checklist

#### ก่อน Release
- [ ] ทดสอบบนอุปกรณ์จริงหลายรุ่น
- [ ] ทดสอบ different screen sizes
- [ ] ทดสอบ orientation changes
- [ ] ทดสอบ low memory scenarios
- [ ] ทดสอบ slow/no internet
- [ ] ตรวจสอบ memory leaks
- [ ] ตรวจสอบ crash reports
- [ ] ทดสอบ deep links (ถ้ามี)
- [ ] ทดสอบ notifications (ถ้ามี)
- [ ] ทดสอบ permissions

#### Performance
- [ ] App launch time < 5 seconds
- [ ] UI smooth 60 FPS
- [ ] Network calls efficient
- [ ] Images optimized
- [ ] No ANR (Application Not Responding)

### 9. Analytics & Crash Reporting

#### Firebase Analytics
```kotlin
// Initialize Firebase
FirebaseAnalytics.getInstance(this)

// Log events
firebaseAnalytics.logEvent("button_click") {
    param("button_name", "login")
    param("screen", "LoginActivity")
}
```

#### Crashlytics
```kotlin
// Log custom messages
FirebaseCrashlytics.getInstance().log("User clicked button")

// Set custom keys
FirebaseCrashlytics.getInstance().setCustomKey("user_id", userId)

// Log non-fatal errors
try {
    // code
} catch (e: Exception) {
    FirebaseCrashlytics.getInstance().recordException(e)
}
```

## 📝 Summary

### เรียนรู้อะไรบ้างใน 2 วัน?

#### Day 1:
- ✅ Kotlin fundamentals
- ✅ Android Activity lifecycle
- ✅ UI Components & Layouts
- ✅ Event handling
- ✅ Calculator app

#### Day 2:
- ✅ Data storage (SharedPreferences, Room)
- ✅ RecyclerView
- ✅ Networking & APIs
- ✅ MVVM Architecture
- ✅ Complete Todo app
- ✅ Publishing process

### ก้าวต่อไป

#### เรียนรู้เพิ่มเติม:
1. **Jetpack Compose** - Modern UI toolkit
2. **Dependency Injection** - Hilt/Dagger
3. **Testing** - Unit tests, UI tests
4. **CI/CD** - Automated builds
5. **Advanced topics** - Custom views, Animations, Sensors

#### Resources:
- [Android Developers](https://developer.android.com/)
- [Kotlin Documentation](https://kotlinlang.org/docs/)
- [Google Codelabs](https://codelabs.developers.google.com/)
- [Android Weekly](https://androidweekly.net/)
- [Medium - Android Dev](https://medium.com/androiddevelopers)

## 🎉 Congratulations!

คุณได้เรียนจบคอร์ส Kotlin Android ภายใน 2 วันแล้ว!

ตอนนี้คุณสามารถ:
- สร้าง Android app ด้วย Kotlin ได้
- ออกแบบ UI ที่สวยงาม
- จัดการข้อมูลและเชื่อมต่อ API
- เผยแพร่แอพบน Play Store

**Keep coding and happy developing! 🚀**

---

## 📞 Contact & Support

หากมีคำถามหรือต้องการความช่วยเหลือ:
- GitHub Issues
- Email: support@example.com
- Community Forum

## 📄 Certificate

ผู้ที่เรียนจบคอร์สจะได้รับใบประกาศนียบัตร (ถ้ามี)
