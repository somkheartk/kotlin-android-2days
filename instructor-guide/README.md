# คู่มือสำหรับผู้สอน

## 🎓 ภาพรวม

คู่มือนี้สำหรับผู้สอนคอร์ส Kotlin Android ภายใน 2 วัน เพื่อช่วยให้การสอนเป็นไปอย่างราบรื่นและมีประสิทธิภาพ

## 📋 การเตรียมตัวก่อนสอน

### ความต้องการขั้นต่ำ

#### สำหรับผู้สอน:
- ✅ เข้าใจ Kotlin และ Android development
- ✅ ประสบการณ์พัฒนาแอพอย่างน้อย 1 ปี
- ✅ เตรียม demo apps และตัวอย่างให้พร้อม

#### สำหรับผู้เรียน:
- ✅ Laptop/Computer (Windows, Mac, หรือ Linux)
- ✅ RAM อย่างน้อย 8GB (แนะนำ 16GB)
- ✅ พื้นที่ว่างอย่างน้อย 10GB
- ✅ มีความรู้พื้นฐานการเขียนโปรแกรม

### เครื่องมือที่ต้องติดตั้ง

1. **Android Studio** (Latest stable version)
2. **JDK 11 or higher**
3. **Android SDK** (API 24-34)
4. **Android Emulator** หรือ Physical device

### การเตรียมสภาพแวดล้อม

ส่ง email ให้ผู้เรียนติดตั้งล่วงหน้า:

```
เรียน ผู้เข้าอบรมทุกท่าน

กรุณาเตรียมเครื่องของท่านดังนี้:

1. ติดตั้ง Android Studio จาก: https://developer.android.com/studio
2. รัน Android Studio ครั้งแรกและดาวน์โหลด SDK
3. สร้าง Virtual Device (Emulator) อย่างน้อย 1 เครื่อง
4. ทดสอบสร้างโปรเจคใหม่เพื่อตรวจสอบว่าใช้งานได้

หากมีปัญหาโปรดแจ้งล่วงหน้า

ขอบคุณครับ/ค่ะ
```

## 📅 ตารางสอนแนะนำ

### Day 1 (8 ชั่วโมง)

#### 09:00 - 09:30: Registration & Setup (30 นาที)
- ตรวจสอบเครื่องผู้เรียนทุกคน
- ช่วยแก้ปัญหาการติดตั้ง
- แจกเอกสารประกอบ

#### 09:30 - 10:00: Introduction (30 นาที)
- แนะนำคอร์ส
- วัตถุประสงค์การเรียนรู้
- ตารางเรียน
- Q&A

#### 10:00 - 12:00: Session 1 - Kotlin Basics (2 ชั่วโมง)
**เนื้อหา:**
- Kotlin overview
- Variables, data types
- Control flow
- Functions
- Collections
- Classes & Objects

**กิจกรรม:**
- Live coding demonstrations
- แบบฝึกหัด: BMI Calculator, Student Management

**Tips:**
- ให้เวลาผู้เรียนลองเขียนตาม
- ตรวจแบบฝึกหัดเป็นระยะ
- ตอบคำถามทันที

#### 12:00 - 13:00: Lunch Break (1 ชั่วโมง)

#### 13:00 - 16:00: Session 2 - Android Basics (3 ชั่วโมง)
**เนื้อหา:**
- Android Studio tour
- Project structure
- Activity lifecycle
- Resources & Gradle
- First app: Hello Kotlin

**กิจกรรม:**
- สร้างโปรเจคแรก
- อธิบาย lifecycle ด้วยภาพ
- แบบฝึกหัด: Counter App

**Tips:**
- ใช้เวลาอธิบาย project structure ให้ชัดเจน
- แสดง Logcat และการ debug
- เน้นย้ำ lifecycle ให้เข้าใจ

#### 16:00 - 16:15: Break (15 นาที)

#### 16:15 - 18:15: Session 3 & 4 - UI & Practice (2 ชั่วโมง)
**เนื้อหา:**
- Layouts (Linear, Constraint, Relative)
- Common widgets
- Event handling
- Calculator project

**กิจกรรม:**
- สร้าง Login screen
- สร้าง Calculator app (hands-on)

**Tips:**
- แสดง Layout Inspector
- อธิบาย constraint ให้เข้าใจ
- ให้เวลาทำ Calculator ด้วยตัวเอง

#### 18:15 - 18:30: Day 1 Recap & Q&A

### Day 2 (8 ชั่วโมง)

#### 09:00 - 09:15: Day 1 Review (15 นาที)
- ทบทวนสิ่งที่เรียนวันแรก
- ตอบคำถามค้างคา

#### 09:15 - 12:00: Session 5 - Data & Storage (2.75 ชั่วโมง)
**เนื้อหา:**
- SharedPreferences
- Data classes & Lists
- RecyclerView
- Todo List project

**กิจกรรม:**
- สร้าง Todo app พื้นฐาน
- เพิ่ม RecyclerView

**Tips:**
- อธิบาย RecyclerView ทีละขั้นตอน
- แสดงการทำงานของ Adapter
- ให้เวลาทำ hands-on

#### 12:00 - 13:00: Lunch Break

#### 13:00 - 16:00: Session 6 - Networking (3 ชั่วโมง)
**เนื้อหา:**
- Retrofit setup
- API calls
- Coroutines
- Weather app

**กิจกรรม:**
- เชื่อมต่อ JSONPlaceholder API
- สร้าง Weather app

**Tips:**
- อธิบาย async programming
- แสดง Network Inspector
- ให้ดู API response ใน Postman

#### 16:00 - 16:15: Break

#### 16:15 - 18:00: Session 7 - Complete Project (1.75 ชั่วโมง)
**เนื้อหา:**
- MVVM architecture
- Room database
- Complete TaskMaster app

**กิจกรรม:**
- สร้างแอพสมบูรณ์

**Tips:**
- อธิบาย architecture pattern
- แสดง Database Inspector
- ให้ผู้เรียนเพิ่มฟีเจอร์เอง

#### 18:00 - 18:30: Session 8 - Publishing & Closing
**เนื้อหา:**
- Build APK/AAB
- Play Store process
- Best practices
- Next steps

**กิจกรรม:**
- Build release APK
- แจกใบประกาศนียบัตร (ถ้ามี)
- รับ feedback

## 💡 เทคนิคการสอน

### 1. Live Coding
- เขียนโค้ดทีละบรรทัดพร้อมอธิบาย
- ให้ผู้เรียนลองเขียนตาม
- ทำผิดบ้างเพื่อแสดงวิธีแก้ไข

### 2. Q&A Sessions
- เปิดโอกาสให้ถามตลอดเวลา
- ตอบคำถามทันที
- ถ้าไม่แน่ใจให้หาคำตอบและตอบทีหลัง

### 3. Hands-on Practice
- ให้เวลาลงมือทำเพียงพอ
- เดินไปช่วยผู้เรียนแต่ละคน
- แก้ปัญหาที่เจอร่วมกัน

### 4. Pair Programming
- จับคู่ผู้เรียน
- ให้ช่วยกันแก้ปัญหา
- สลับบทบาท

### 5. Code Review
- Review code ของผู้เรียน
- ชี้จุดที่ควรปรับปรุง
- แสดงวิธีเขียนที่ดีกว่า

## 🚨 ปัญหาที่พบบ่อย

### ปัญหาการติดตั้ง

**ปัญหา:** Android Studio ช้า
**แก้:** 
- เพิ่ม heap size
- ปิด antivirus ชั่วคราว
- ใช้ SSD

**ปัญหา:** Emulator ไม่เริ่มทำงาน
**แก้:**
- Enable virtualization ใน BIOS
- ลองใช้ physical device แทน
- ลด RAM ของ emulator

### ปัญหาระหว่างเรียน

**ปัญหา:** Gradle build ล้มเหลว
**แก้:**
- Sync project with Gradle files
- Clear cache and restart
- Check internet connection

**ปัญหา:** Import errors
**แก้:**
- Invalidate caches / Restart
- Sync Gradle
- Check dependencies version

**ปัญหา:** App crashes
**แก้:**
- ดู Logcat
- หา stack trace
- แก้ตาม error message

## 📊 การประเมินผล

### วัดความเข้าใจ
1. แบบทดสอบก่อนเรียน (Pre-test)
2. แบบฝึกหัดในแต่ละ session
3. โปรเจคสุดท้าย
4. แบบทดสอบหลังเรียน (Post-test)

### เกณฑ์การผ่าน
- เข้าเรียนครบ 80%
- ส่งแบบฝึกหัดทุกข้อ
- ทำโปรเจคสุดท้ายสำเร็จ

## 📝 Checklist ก่อนสอน

### 1 สัปดาห์ก่อน:
- [ ] ส่ง email แจ้งผู้เรียน
- [ ] เตรียมเอกสารประกอบ
- [ ] ทดสอบ demo apps
- [ ] เตรียมแบบฝึกหัด

### 1 วันก่อน:
- [ ] ตรวจสอบอุปกรณ์
- [ ] ทดสอบโปรเจคเตอร์
- [ ] เตรียม backup slides
- [ ] พิมพ์เอกสาร

### วันสอน:
- [ ] มาก่อนเวลา 30 นาที
- [ ] Setup ห้องเรียน
- [ ] ทดสอบ internet
- [ ] เปิด Android Studio

## 🎯 เป้าหมายการสอน

ผู้เรียนควรสามารถ:
- ✅ เขียน Kotlin ขั้นพื้นฐานได้
- ✅ สร้าง Android app ได้ด้วยตนเอง
- ✅ ออกแบบ UI ที่ใช้งานได้
- ✅ จัดการข้อมูลและ API
- ✅ มีความมั่นใจในการพัฒนาต่อ

## 📞 Support

หลังจากจบคอร์ส:
- ตั้ง LINE/Discord group
- ตอบคำถามต่อเนื่อง 1-2 สัปดาห์
- แนะนำแหล่งเรียนรู้เพิ่มเติม

---

**Good luck with your teaching! 🎓**
