# Contributing Guide - คู่มือการมีส่วนร่วม

ขอบคุณที่สนใจมีส่วนร่วมในการพัฒนาคอร์สนี้! 🎉

## 🤝 วิธีการมีส่วนร่วม

### 1. รายงานปัญหา (Issues)

หากพบปัญหาหรือข้อผิดพลาด:

1. เช็คว่ามี issue คล้ายๆ อยู่แล้วหรือไม่
2. เปิด new issue
3. ใส่รายละเอียด:
   - ปัญหาที่พบ
   - ขั้นตอนการทำซ้ำ
   - สภาพแวดล้อม (OS, Android Studio version)
   - Screenshot (ถ้ามี)

### 2. แนะนำปรับปรุง (Suggestions)

หากมีไอเดียปรับปรุงคอร์ส:

1. เปิด issue ใหม่
2. ใช้ tag: `enhancement`
3. อธิบายไอเดีย:
   - ต้องการอะไร
   - ทำไมจำเป็น
   - มีตัวอย่างไหม

### 3. ส่ง Pull Request

#### ขั้นตอน:

1. **Fork repository**
   ```bash
   # คลิก Fork บน GitHub
   ```

2. **Clone repository**
   ```bash
   git clone https://github.com/YOUR_USERNAME/kotlin-android-2days.git
   cd kotlin-android-2days
   ```

3. **สร้าง branch ใหม่**
   ```bash
   git checkout -b feature/your-feature-name
   # หรือ
   git checkout -b fix/bug-description
   ```

4. **ทำการแก้ไข**
   - แก้ไขไฟล์ที่ต้องการ
   - ทดสอบให้แน่ใจว่าใช้งานได้

5. **Commit changes**
   ```bash
   git add .
   git commit -m "Add: feature description"
   # หรือ
   git commit -m "Fix: bug description"
   ```

6. **Push to GitHub**
   ```bash
   git push origin feature/your-feature-name
   ```

7. **Create Pull Request**
   - ไปที่ GitHub repository
   - คลิก "New Pull Request"
   - เลือก branch ที่สร้าง
   - อธิบายการเปลี่ยนแปลง
   - Submit

## 📝 แนวทางการเขียน

### เนื้อหาการสอน

1. **ใช้ภาษาไทยที่เข้าใจง่าย**
   - หลีกเลี่ยงศัพท์เทคนิคมากเกินไป
   - อธิบายให้เข้าใจง่าย
   - ใช้ตัวอย่างที่เกี่ยวข้อง

2. **โครงสร้างเนื้อหา**
   ```markdown
   # Session Title
   
   ## 🎯 วัตถุประสงค์
   
   ## 📚 เนื้อหา
   
   ### หัวข้อย่อย
   
   ## 💻 แบบฝึกหัด
   
   ## 🔗 Resources
   ```

3. **Code Examples**
   - ใส่ comments เป็นภาษาไทย
   - ใช้ชื่อตัวแปรที่มีความหมาย
   - Format code ให้สวยงาม
   
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

### การเพิ่มตัวอย่างโค้ด

1. **ทดสอบให้แน่ใจว่าใช้งานได้**
2. **ใส่ comment อธิบาย**
3. **ใช้ best practices**
4. **เช็ค coding style**

### การเพิ่มแบบฝึกหัด

1. **มีวัตถุประสงค์ชัดเจน**
2. **ระดับความยากเหมาะสม**
3. **มีเฉลยด้วย**
4. **เกี่ยวข้องกับเนื้อหา**

## 🎨 Style Guidelines

### Markdown

- ใช้ heading level ตามลำดับ
- ใส่ blank line ระหว่าง sections
- ใช้ emoji ให้เหมาะสม
- Format code blocks ด้วย syntax highlighting

### Code Style

```kotlin
// Kotlin coding conventions
class MyClass {
    private val property: String = "value"
    
    fun myFunction(parameter: Int): String {
        return "result"
    }
}
```

### Naming Conventions

- **Files**: `kebab-case.md`
- **Folders**: `kebab-case`
- **Code**: Kotlin conventions

## 🐛 การรายงาน Bug

Template สำหรับรายงาน bug:

```markdown
## อธิบายปัญหา
[อธิบายปัญหาที่พบ]

## ขั้นตอนการทำซ้ำ
1. ไปที่ '...'
2. คลิกที่ '....'
3. Scroll ลงไปที่ '....'
4. เห็น error

## พฤติกรรมที่คาดหวัง
[อธิบายสิ่งที่คาดว่าจะเกิด]

## Screenshots
[ถ้ามี ใส่ screenshot]

## สภาพแวดล้อม
- OS: [e.g. Windows 10]
- Android Studio: [e.g. Hedgehog 2023.1.1]
- Kotlin: [e.g. 1.9.0]
```

## ✨ Feature Request

Template สำหรับแนะนำฟีเจอร์:

```markdown
## ฟีเจอร์ที่ต้องการ
[อธิบายฟีเจอร์]

## ทำไมต้องการ
[อธิบายเหตุผล]

## ทางเลือกอื่นๆ
[มีทางเลือกอื่นไหม]

## ข้อมูลเพิ่มเติม
[ข้อมูลหรือ context อื่นๆ]
```

## 🎯 Priority Tags

- `critical` - ปัญหาร้ายแรง ต้องแก้ด่วน
- `bug` - ข้อผิดพลาด
- `enhancement` - ปรับปรุง
- `documentation` - เกี่ยวกับเอกสาร
- `help wanted` - ต้องการความช่วยเหลือ
- `good first issue` - เหมาะสำหรับผู้เริ่มต้น

## 👥 Code Review Process

เมื่อส่ง Pull Request:

1. **Maintainer จะ review**
2. **อาจมี comments หรือ suggestions**
3. **แก้ไขตาม feedback**
4. **รอ approval**
5. **Merge เข้า main branch**

## 📋 Checklist ก่อน Submit PR

- [ ] ทดสอบโค้ดแล้ว
- [ ] เพิ่ม/แก้ไข documentation
- [ ] ตรวจสอบ spelling และ grammar
- [ ] Format โค้ดให้เรียบร้อย
- [ ] ไม่มี console.log หรือ debug code ค้างอยู่
- [ ] Commit message ชัดเจน

## 🌟 ประเภทการมีส่วนร่วม

ไม่ใช่แค่โค้ด! คุณสามารถช่วย:

- 📝 เขียน/แก้ไขเอกสาร
- 🐛 รายงาน bugs
- 💡 แนะนำฟีเจอร์
- 🎨 ปรับปรุง UI/UX
- 🌐 แปลภาษา
- 📹 สร้าง video tutorials
- 💬 ตอบคำถามใน issues
- ⭐ Star repository
- 🔗 แชร์ให้คนอื่นรู้จัก

## 📞 Contact

หากมีคำถาม:
- เปิด issue บน GitHub
- Email: support@example.com

## 📜 Code of Conduct

- ใจเย็น เคารพซึ่งกันและกัน
- ไม่ใช้ถ้อยคำหยาบคาย
- รับฟังความคิดเห็นผู้อื่น
- ช่วยเหลือผู้เริ่มต้น
- สร้างสิ่งดีๆ ร่วมกัน

## 🙏 ขอบคุณ

ขอบคุณทุกๆ คนที่มีส่วนร่วม! 

Contributors:
- [List of contributors](https://github.com/somkheartk/kotlin-android-2days/graphs/contributors)

---

**Happy Contributing! 🎉**
