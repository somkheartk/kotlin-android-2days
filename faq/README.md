# FAQ - คำถามที่พบบ่อย

## 📱 ทั่วไป

### Q: ต้องมีพื้นฐานอะไรบ้างก่อนเรียน?
**A:** 
- มีความรู้พื้นฐานการเขียนโปรแกรม (ภาษาใดก็ได้)
- เข้าใจ concept พื้นฐาน: variables, loops, functions
- ไม่จำเป็นต้องเคยเขียน Java หรือ Kotlin มาก่อน

### Q: ใช้เวลาเรียนจริงนานแค่ไหน?
**A:** 2 วัน (รวม 16 ชั่วโมง)
- วันละ 8 ชั่วโมง (09:00-18:00)
- มีพักกลางวัน 1 ชั่วโมง
- มีพักสั้นๆ 2-3 ครั้ง

### Q: ได้แอพจริงหรือเปล่า?
**A:** ใช่! จะได้แอพหลายตัว:
- Calculator App
- Todo List App
- Weather App
- TaskMaster App (โปรเจคหลัก)

### Q: หลังเรียนจบสามารถทำอะไรได้บ้าง?
**A:** 
- สร้างแอพ Android พื้นฐานได้เอง
- อ่านและเข้าใจ code Kotlin
- ต่อยอดเรียนรู้ได้ด้วยตัวเอง
- เตรียมพร้อมสำหรับการทำงานจริง

## 💻 เกี่ยวกับเครื่องมือ

### Q: ต้องใช้ Mac หรือเปล่า?
**A:** ไม่จำเป็น สามารถใช้ได้:
- Windows 10/11
- macOS
- Linux (Ubuntu, etc.)

### Q: คอมพิวเตอร์ต้อง spec อะไร?
**A:** ขั้นต่ำ:
- RAM: 8GB (แนะนำ 16GB)
- Disk: 10GB ว่าง (แนะนำ SSD)
- Processor: Intel i5 หรือเทียบเท่า
- Internet: สำหรับดาวน์โหลด SDK

### Q: ต้องมีมือถือ Android หรือเปล่า?
**A:** ไม่จำเป็น
- ใช้ Emulator ได้
- แต่ถ้ามีจริงจะทดสอบได้เร็วกว่า

### Q: Android Studio ใช้พื้นที่เท่าไหร่?
**A:** ประมาณ:
- Android Studio: 2-3 GB
- Android SDK: 5-10 GB
- Emulator images: 2-5 GB แต่ละตัว

## 📚 เกี่ยวกับเนื้อหา

### Q: สอน Jetpack Compose ด้วยหรือไม่?
**A:** คอร์สนี้เน้น XML-based UI
- Jetpack Compose เป็น topic ขั้นสูง
- แนะนำให้เรียน XML ก่อน
- Compose เรียนต่อได้ในภายหลัง

### Q: มีสอน Kotlin Multiplatform ไหม?
**A:** ไม่มี คอร์สนี้เน้น Android เท่านั้น

### Q: สอน Java ด้วยหรือไม่?
**A:** ไม่ เน้น Kotlin เท่านั้น
- Kotlin เป็นภาษาหลักของ Android
- เขียนง่ายกว่า Java
- Google แนะนำให้ใช้ Kotlin

### Q: มีสอนการทำแอพเกมไหม?
**A:** ไม่มี คอร์สนี้เน้น:
- Business apps
- Utility apps
- CRUD applications

## 🔧 ปัญหาทางเทคนิค

### Q: Gradle build ช้ามาก ทำยังไง?
**A:** วิธีแก้:
```kotlin
// เพิ่มใน gradle.properties
org.gradle.jvmargs=-Xmx2048m -XX:MaxPermSize=512m
org.gradle.parallel=true
org.gradle.caching=true
```

### Q: Emulator ช้า/ไม่เริ่มทำงาน?
**A:** ลองวิธีนี้:
1. Enable Virtualization ใน BIOS
2. Install HAXM (Intel) หรือ Hyper-V (Windows)
3. ลด RAM ของ emulator
4. หรือใช้ physical device แทน

### Q: Import ไม่เจอ class?
**A:** ลองทำตามนี้:
1. File → Invalidate Caches / Restart
2. Tools → Android → Sync Project with Gradle Files
3. Build → Clean Project
4. Build → Rebuild Project

### Q: แอพ crash ทันทีที่เปิด?
**A:** ตรวจสอบ:
1. ดู Logcat หา error
2. ตรวจสอบ AndroidManifest.xml
3. ตรวจสอบ permissions
4. ตรวจสอบ minSdk version

### Q: Error: "SDK location not found"?
**A:** แก้ไข:
1. สร้างไฟล์ `local.properties`
2. เพิ่ม: `sdk.dir=/path/to/Android/sdk`
3. Sync Gradle

## 💰 เกี่ยวกับค่าใช้จ่าย

### Q: ต้องเสียเงินไหมในการเรียน?
**A:** ขึ้นอยู่กับผู้จัด
- คอร์สนี้เป็น open source (ฟรี)
- ถ้ามีผู้สอนอาจมีค่าใช้จ่าย

### Q: เผยแพร่แอพบน Play Store เสียเงินไหม?
**A:** ใช่
- ค่าสมัคร Developer Account: $25 (ครั้งเดียว)
- หลังจากนั้นไม่มีค่าใช้จ่ายประจำ

### Q: ต้องซื้อ library หรือ tools อะไรเพิ่มไหม?
**A:** ไม่ต้อง
- ทุกอย่างที่ใช้เป็น free และ open source
- Android Studio ฟรี
- Libraries ที่ใช้ฟรีทั้งหมด

## 🎓 เกี่ยวกับการเรียนรู้

### Q: เรียนออนไลน์ได้ไหม?
**A:** ได้ 
- เนื้อหาทั้งหมดอยู่บน GitHub
- สามารถเรียนด้วยตัวเองได้
- แต่ไม่มีผู้สอนตอบคำถาม

### Q: เรียนไม่ทัน ทำยังไง?
**A:** 
- เนื้อหาอยู่ที่นี่ตลอด สามารถกลับมาอ่านได้
- ทำแบบฝึกหัดช้าๆ ที่บ้าน
- ถามใน GitHub Issues

### Q: อยากเรียนเพิ่มต้องทำยังไง?
**A:** แนะนำ:
- [Android Developers](https://developer.android.com/)
- [Kotlin Koans](https://kotlinlang.org/docs/koans.html)
- [Udacity Android Course](https://www.udacity.com/course/developing-android-apps-with-kotlin--ud9012)
- [YouTube - Coding in Flow](https://www.youtube.com/c/CodinginFlow)

### Q: มีคนสอนช่วยไหม?
**A:** ขึ้นอยู่กับ:
- ถ้าเข้าคอร์สแบบมีผู้สอน: มี
- ถ้าเรียนเอง: ถามใน community ได้

## 📱 เกี่ยวกับการพัฒนาจริง

### Q: ทำแอพขายได้เลยไหม?
**A:** ได้ แต่ควร:
- ทดสอบให้ดี
- ปรับปรุง UI/UX
- เพิ่ม error handling
- ทำ privacy policy
- ปฏิบัติตาม Play Store policies

### Q: จะได้งานทำหรือเปล่า?
**A:** คอร์สนี้เป็นพื้นฐาน
- ต้องเรียนรู้เพิ่มเติม
- สร้าง portfolio
- ฝึกฝนต่อเนื่อง
- อาจต้อง 3-6 เดือนเพื่อพร้อมทำงาน

### Q: ควรเรียนอะไรต่อ?
**A:** แนะนำ:
1. Jetpack Compose (Modern UI)
2. Dependency Injection (Hilt)
3. Testing (Unit & UI tests)
4. Architecture Patterns (Clean Architecture)
5. CI/CD

### Q: แอพที่สร้างใช้ได้จริงหรือไม่?
**A:** ใช่ แต่เป็นแอพพื้นฐาน
- เหมาะสำหรับเรียนรู้
- สามารถต่อยอดได้
- ต้องปรับปรุงก่อนเผยแพร่จริง

## 🐛 Debug & Troubleshooting

### Q: ไม่เห็น Logcat?
**A:** 
- View → Tool Windows → Logcat
- หรือ Alt + 6 (Windows) / Cmd + 6 (Mac)

### Q: ไม่เห็นปุ่ม Run?
**A:** 
- ต้องเลือก configuration ก่อน
- คลิกที่ dropdown ข้าง Run button

### Q: แก้โค้ดแล้วยังเป็นเหมือนเดิม?
**A:** 
- Build → Clean Project
- Build → Rebuild Project
- หรือ Run อีกครั้ง

### Q: Cannot resolve symbol 'R'?
**A:** 
1. Check for XML errors
2. Sync Gradle
3. Clean and Rebuild
4. Invalidate Caches

## 📞 ติดต่อ

### มีคำถามเพิ่มเติม?
- เปิด GitHub Issue
- ถามใน Discussion
- Email: support@example.com

---

**หากไม่พบคำตอบที่ต้องการ กรุณาเปิด Issue บน GitHub**
