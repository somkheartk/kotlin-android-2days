# Session 6: Networking & APIs (3 ชั่วโมง)

## 🎯 วัตถุประสงค์
เรียนรู้การเชื่อมต่อ API และดึงข้อมูลจาก Internet

## 📚 เนื้อหา

### 1. เพิ่ม Permission (10 นาที)

**AndroidManifest.xml**
```xml
<manifest ...>
    <!-- Internet Permission -->
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    
    <application
        android:usesCleartextTraffic="true"
        ...>
    </application>
</manifest>
```

### 2. Retrofit Setup (30 นาที)

Retrofit เป็น library ยอดนิยมสำหรับเชื่อมต่อ REST API

#### เพิ่ม Dependencies

**build.gradle.kts (Module)**
```kotlin
dependencies {
    // Retrofit
    implementation("com.squareup.retrofit2:retrofit:2.9.0")
    implementation("com.squareup.retrofit2:converter-gson:2.9.0")
    
    // Gson
    implementation("com.google.code.gson:gson:2.10.1")
    
    // OkHttp (Logging)
    implementation("com.squareup.okhttp3:logging-interceptor:4.11.0")
    
    // Coroutines
    implementation("org.jetbrains.kotlinx:kotlinx-coroutines-android:1.7.3")
    implementation("androidx.lifecycle:lifecycle-runtime-ktx:2.6.2")
}
```

### 3. สร้าง Data Models (20 นาที)

ใช้ JSONPlaceholder API สำหรับทดสอบ: https://jsonplaceholder.typicode.com/

#### Post Model
```kotlin
data class Post(
    val userId: Int,
    val id: Int,
    val title: String,
    val body: String
)
```

#### User Model
```kotlin
data class User(
    val id: Int,
    val name: String,
    val username: String,
    val email: String,
    val phone: String,
    val website: String
)
```

#### Weather Model (OpenWeatherMap API)
```kotlin
data class WeatherResponse(
    val weather: List<Weather>,
    val main: Main,
    val name: String
)

data class Weather(
    val id: Int,
    val main: String,
    val description: String,
    val icon: String
)

data class Main(
    val temp: Double,
    val feels_like: Double,
    val temp_min: Double,
    val temp_max: Double,
    val humidity: Int
)
```

### 4. สร้าง API Interface (30 นาที)

**ApiService.kt**
```kotlin
interface ApiService {
    
    @GET("posts")
    suspend fun getPosts(): List<Post>
    
    @GET("posts/{id}")
    suspend fun getPost(@Path("id") postId: Int): Post
    
    @GET("users")
    suspend fun getUsers(): List<User>
    
    @POST("posts")
    suspend fun createPost(@Body post: Post): Post
    
    @PUT("posts/{id}")
    suspend fun updatePost(@Path("id") id: Int, @Body post: Post): Post
    
    @DELETE("posts/{id}")
    suspend fun deletePost(@Path("id") id: Int): Response<Unit>
}
```

**WeatherApiService.kt**
```kotlin
interface WeatherApiService {
    
    @GET("weather")
    suspend fun getCurrentWeather(
        @Query("q") city: String,
        @Query("appid") apiKey: String,
        @Query("units") units: String = "metric",
        @Query("lang") lang: String = "th"
    ): WeatherResponse
}
```

### 5. สร้าง Retrofit Instance (20 นาที)

**RetrofitClient.kt**
```kotlin
object RetrofitClient {
    
    private const val BASE_URL = "https://jsonplaceholder.typicode.com/"
    private const val WEATHER_BASE_URL = "https://api.openweathermap.org/data/2.5/"
    
    private val loggingInterceptor = HttpLoggingInterceptor().apply {
        level = HttpLoggingInterceptor.Level.BODY
    }
    
    private val okHttpClient = OkHttpClient.Builder()
        .addInterceptor(loggingInterceptor)
        .connectTimeout(30, TimeUnit.SECONDS)
        .readTimeout(30, TimeUnit.SECONDS)
        .build()
    
    private val retrofit = Retrofit.Builder()
        .baseUrl(BASE_URL)
        .client(okHttpClient)
        .addConverterFactory(GsonConverterFactory.create())
        .build()
    
    private val weatherRetrofit = Retrofit.Builder()
        .baseUrl(WEATHER_BASE_URL)
        .client(okHttpClient)
        .addConverterFactory(GsonConverterFactory.create())
        .build()
    
    val apiService: ApiService = retrofit.create(ApiService::class.java)
    val weatherService: WeatherApiService = weatherRetrofit.create(WeatherApiService::class.java)
}
```

### 6. Coroutines สำหรับ Async Operations (30 นาที)

```kotlin
class PostsActivity : AppCompatActivity() {
    
    private val apiService = RetrofitClient.apiService
    
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_posts)
        
        // Fetch data
        fetchPosts()
    }
    
    private fun fetchPosts() {
        // Launch coroutine in lifecycle scope
        lifecycleScope.launch {
            try {
                // Show loading
                showLoading(true)
                
                // Network call (on IO thread)
                val posts = withContext(Dispatchers.IO) {
                    apiService.getPosts()
                }
                
                // Update UI (on Main thread)
                displayPosts(posts)
                
            } catch (e: Exception) {
                // Handle error
                handleError(e)
            } finally {
                // Hide loading
                showLoading(false)
            }
        }
    }
    
    private fun showLoading(isLoading: Boolean) {
        // Show/hide progress bar
    }
    
    private fun displayPosts(posts: List<Post>) {
        // Update RecyclerView
    }
    
    private fun handleError(exception: Exception) {
        Toast.makeText(
            this,
            "Error: ${exception.message}",
            Toast.LENGTH_LONG
        ).show()
    }
}
```

### 7. สร้าง Weather App (60 นาที)

#### activity_weather.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<ScrollView
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="#87CEEB">

    <androidx.constraintlayout.widget.ConstraintLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:padding="24dp">

        <!-- Search Bar -->
        <LinearLayout
            android:id="@+id/layoutSearch"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:orientation="horizontal"
            app:layout_constraintTop_toTopOf="parent"
            app:layout_constraintStart_toStartOf="parent"
            app:layout_constraintEnd_toEndOf="parent">

            <EditText
                android:id="@+id/editCity"
                android:layout_width="0dp"
                android:layout_height="wrap_content"
                android:layout_weight="1"
                android:hint="ชื่อเมือง"
                android:inputType="text"
                android:background="@android:color/white"
                android:padding="12dp" />

            <Button
                android:id="@+id/btnSearch"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="ค้นหา"
                android:layout_marginStart="8dp" />
        </LinearLayout>

        <!-- Loading -->
        <ProgressBar
            android:id="@+id/progressBar"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:visibility="gone"
            app:layout_constraintTop_toBottomOf="@id/layoutSearch"
            app:layout_constraintStart_toStartOf="parent"
            app:layout_constraintEnd_toEndOf="parent"
            android:layout_marginTop="32dp" />

        <!-- Weather Content -->
        <LinearLayout
            android:id="@+id/layoutContent"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:orientation="vertical"
            android:gravity="center"
            android:visibility="gone"
            app:layout_constraintTop_toBottomOf="@id/layoutSearch"
            app:layout_constraintStart_toStartOf="parent"
            app:layout_constraintEnd_toEndOf="parent"
            android:layout_marginTop="32dp">

            <!-- City Name -->
            <TextView
                android:id="@+id/tvCityName"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="กรุงเทพฯ"
                android:textSize="32sp"
                android:textStyle="bold"
                android:textColor="@android:color/white" />

            <!-- Weather Icon -->
            <ImageView
                android:id="@+id/ivWeatherIcon"
                android:layout_width="120dp"
                android:layout_height="120dp"
                android:layout_marginTop="16dp"
                android:contentDescription="Weather Icon" />

            <!-- Temperature -->
            <TextView
                android:id="@+id/tvTemperature"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="30°C"
                android:textSize="72sp"
                android:textStyle="bold"
                android:textColor="@android:color/white"
                android:layout_marginTop="16dp" />

            <!-- Description -->
            <TextView
                android:id="@+id/tvDescription"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="มีเมฆบางส่วน"
                android:textSize="24sp"
                android:textColor="@android:color/white"
                android:layout_marginTop="8dp" />

            <!-- Details Card -->
            <androidx.cardview.widget.CardView
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:layout_marginTop="32dp"
                app:cardCornerRadius="16dp"
                app:cardElevation="8dp">

                <LinearLayout
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    android:orientation="horizontal"
                    android:padding="24dp">

                    <!-- Feels Like -->
                    <LinearLayout
                        android:layout_width="0dp"
                        android:layout_height="wrap_content"
                        android:layout_weight="1"
                        android:orientation="vertical"
                        android:gravity="center">

                        <TextView
                            android:layout_width="wrap_content"
                            android:layout_height="wrap_content"
                            android:text="รู้สึกเหมือน"
                            android:textSize="14sp"
                            android:textColor="#666666" />

                        <TextView
                            android:id="@+id/tvFeelsLike"
                            android:layout_width="wrap_content"
                            android:layout_height="wrap_content"
                            android:text="32°C"
                            android:textSize="20sp"
                            android:textStyle="bold"
                            android:layout_marginTop="4dp" />
                    </LinearLayout>

                    <!-- Humidity -->
                    <LinearLayout
                        android:layout_width="0dp"
                        android:layout_height="wrap_content"
                        android:layout_weight="1"
                        android:orientation="vertical"
                        android:gravity="center">

                        <TextView
                            android:layout_width="wrap_content"
                            android:layout_height="wrap_content"
                            android:text="ความชื้น"
                            android:textSize="14sp"
                            android:textColor="#666666" />

                        <TextView
                            android:id="@+id/tvHumidity"
                            android:layout_width="wrap_content"
                            android:layout_height="wrap_content"
                            android:text="65%"
                            android:textSize="20sp"
                            android:textStyle="bold"
                            android:layout_marginTop="4dp" />
                    </LinearLayout>

                </LinearLayout>
            </androidx.cardview.widget.CardView>

        </LinearLayout>

    </androidx.constraintlayout.widget.ConstraintLayout>

</ScrollView>
```

#### WeatherActivity.kt
```kotlin
class WeatherActivity : AppCompatActivity() {

    private lateinit var editCity: EditText
    private lateinit var btnSearch: Button
    private lateinit var progressBar: ProgressBar
    private lateinit var layoutContent: LinearLayout
    private lateinit var tvCityName: TextView
    private lateinit var tvTemperature: TextView
    private lateinit var tvDescription: TextView
    private lateinit var tvFeelsLike: TextView
    private lateinit var tvHumidity: TextView
    private lateinit var ivWeatherIcon: ImageView

    private val weatherService = RetrofitClient.weatherService
    private val apiKey = "YOUR_API_KEY" // Get from openweathermap.org

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_weather)

        initViews()
        setupListeners()
        
        // Load default city
        searchWeather("Bangkok")
    }

    private fun initViews() {
        editCity = findViewById(R.id.editCity)
        btnSearch = findViewById(R.id.btnSearch)
        progressBar = findViewById(R.id.progressBar)
        layoutContent = findViewById(R.id.layoutContent)
        tvCityName = findViewById(R.id.tvCityName)
        tvTemperature = findViewById(R.id.tvTemperature)
        tvDescription = findViewById(R.id.tvDescription)
        tvFeelsLike = findViewById(R.id.tvFeelsLike)
        tvHumidity = findViewById(R.id.tvHumidity)
        ivWeatherIcon = findViewById(R.id.ivWeatherIcon)
    }

    private fun setupListeners() {
        btnSearch.setOnClickListener {
            val city = editCity.text.toString().trim()
            if (city.isNotEmpty()) {
                searchWeather(city)
            } else {
                Toast.makeText(this, "กรุณากรอกชื่อเมือง", Toast.LENGTH_SHORT).show()
            }
        }
    }

    private fun searchWeather(city: String) {
        lifecycleScope.launch {
            try {
                showLoading(true)

                val weather = withContext(Dispatchers.IO) {
                    weatherService.getCurrentWeather(city, apiKey)
                }

                displayWeather(weather)

            } catch (e: HttpException) {
                if (e.code() == 404) {
                    Toast.makeText(this@WeatherActivity, "ไม่พบเมืองนี้", Toast.LENGTH_SHORT).show()
                } else {
                    Toast.makeText(this@WeatherActivity, "เกิดข้อผิดพลาด: ${e.message()}", Toast.LENGTH_SHORT).show()
                }
            } catch (e: Exception) {
                Toast.makeText(this@WeatherActivity, "Error: ${e.message}", Toast.LENGTH_SHORT).show()
            } finally {
                showLoading(false)
            }
        }
    }

    private fun displayWeather(weather: WeatherResponse) {
        layoutContent.visibility = View.VISIBLE

        tvCityName.text = weather.name
        tvTemperature.text = "${weather.main.temp.toInt()}°C"
        tvDescription.text = weather.weather[0].description
        tvFeelsLike.text = "${weather.main.feels_like.toInt()}°C"
        tvHumidity.text = "${weather.main.humidity}%"

        // Load weather icon
        val iconCode = weather.weather[0].icon
        loadWeatherIcon(iconCode)
    }

    private fun loadWeatherIcon(iconCode: String) {
        // You can use Glide or Picasso to load image from URL
        // For now, set a default icon based on weather condition
        val iconRes = when {
            iconCode.contains("01") -> R.drawable.ic_sunny
            iconCode.contains("02") -> R.drawable.ic_cloudy
            iconCode.contains("03") -> R.drawable.ic_clouds
            iconCode.contains("04") -> R.drawable.ic_clouds
            iconCode.contains("09") -> R.drawable.ic_rain
            iconCode.contains("10") -> R.drawable.ic_rain
            iconCode.contains("11") -> R.drawable.ic_storm
            iconCode.contains("13") -> R.drawable.ic_snow
            else -> R.drawable.ic_weather
        }
        ivWeatherIcon.setImageResource(iconRes)
    }

    private fun showLoading(isLoading: Boolean) {
        progressBar.visibility = if (isLoading) View.VISIBLE else View.GONE
        btnSearch.isEnabled = !isLoading
    }
}
```

## 💻 แบบฝึกหัด

### Exercise 1: Posts List
สร้างแอพแสดงรายการ posts จาก JSONPlaceholder API

### Exercise 2: User Profile
สร้างหน้าแสดงข้อมูลผู้ใช้จาก API

### Exercise 3: Create Post
สร้างฟอร์มสำหรับสร้าง post ใหม่และส่งไปยัง API

## 📝 Tips

- ใช้ Coroutines สำหรับ async operations
- จัดการ error ให้ดี (try-catch)
- แสดง loading indicator
- ตรวจสอบ internet connection ก่อนเรียก API
- ใช้ sealed class สำหรับ result states

## 🔗 Resources

- [Retrofit Documentation](https://square.github.io/retrofit/)
- [JSONPlaceholder API](https://jsonplaceholder.typicode.com/)
- [OpenWeatherMap API](https://openweathermap.org/api)
- [Kotlin Coroutines](https://kotlinlang.org/docs/coroutines-overview.html)

## ⏭️ Next Session

[Session 7: Complete Project](../session7-complete-project/README.md)
