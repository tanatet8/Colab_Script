# 🔧 Colab_Script
รวมสคริปต์ Python สำหรับ Google Colab - เครื่องมือแปลงข้อความ สกัดสื่อ และแปลงข้อมูล

---

## 📂 วิธีใส่ไฟล์ใน Google Colab

### **วิธีที่ 1: Upload ผ่าน Code (แนะนำ)**
```python
from google.colab import files
uploaded = files.upload()
```
- รันโค้ดแล้วจะมีปุ่ม "Choose Files"
- เลือกไฟล์จากเครื่อง → อัปโหลดอัตโนมัติ
- ไฟล์อยู่ที่ `/content/ชื่อไฟล์`

### **วิธีที่ 2: ลาก Drop ใน File Panel**
1. เปิด File panel ซ้ายมือ (ไอคอนโฟลเดอร์)
2. ลากไฟล์จากเครื่องมาวาง
3. ไฟล์จะอยู่ที่ `/content/ชื่อไฟล์`

### **วิธีที่ 3: Google Drive (สำหรับไฟล์ใหญ่)**
```python
from google.colab import drive
drive.mount('/content/drive')
# ไฟล์อยู่ที่ /content/drive/MyDrive/ชื่อไฟล์
```

### **แล้วรันโค้ดแปลง:**
```python
# ระบุชื่อไฟล์ที่อัปโหลด
filename = "ชื่อไฟล์.pdf"  # เปลี่ยนตามไฟล์จริง
```

---

## 📁 สคริปต์ที่มีให้ใช้

### 📄 แปลง PDF เป็น Text
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/tanatet8/Colab_Script/blob/main/PDF_to_Text_Converter.ipynb)

แปลงไฟล์ PDF เป็นข้อความธรรมดาด้วย `pdfplumber` - เหมาะสำหรับสร้าง text corpus หรือวิเคราะห์ข้อมูล

**ฟีเจอร์:**
- แปลงหลายไฟล์พร้อมกัน
- รองรับ UTF-8 (ภาษาไทย/จีน/อักขระพิเศษ)
- แสดงความคืบหน้าและสถิติไฟล์
- ดาวน์โหลดไฟล์ที่แปลงแล้วอัตโนมัติ
- จัดการไฟล์เสียหายได้

**วิธีใช้:**
1. เปิด notebook ใน Colab
2. **วางไฟล์ e-book ใน Colab:**
   - **วิธี 1 (แนะนำ)**: รัน `files.upload()` แล้วเลือกไฟล์
   - **วิธี 2**: ลากไฟล์มาวางใน File panel (ซ้ายมือ)
   - **วิธี 3**: อัปโหลดใน Google Drive แล้ว mount
3. รันทุก cell
4. ดาวน์โหลดไฟล์ `.txt` ที่แปลงแล้ว

---

### 🎬 สกัด Transcript จาก YouTube
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/tanatet8/Colab_Script/blob/main/youtube_transcript_extractor.ipynb)

ดึงคำบรรยาย (subtitle) จากวิดีโอ YouTube ด้วย `youtube-transcript-api`

**ฟีเจอร์:**
- รองรับหลายภาษา
- จัดรูปแบบคำบรรยายให้เรียบร้อย
- เหมาะสำหรับใส่ LLM หรือวิเคราะห์เนื้อหา
- ส่งออกเป็นไฟล์ `.txt`

**วิธีใช้:**
1. เปิด notebook ใน Colab  
2. เปลี่ยน `video_url`  
3. รันทุก cell และดาวน์โหลด `transcript.txt`

---

## 🛠 วิธีแปลง PDF ที่มี DRM

### **วิธีที่ 1: WebApp (ง่ายที่สุด)**
**PDF24 Tools** - `pdf24.org/pdf-to-text`
- ฟรี ไม่ต้องสมัคร
- รองรับภาษาไทยดี
- ลากไฟล์เข้าไป กด convert เสร็จ

### **วิธีที่ 2: Adobe Digital Editions → WebApp**
1. เปิดไฟล์ใน Adobe Digital Editions
2. File → Print → Microsoft Print to PDF
3. นำ PDF ไปแปลงใน WebApp (PDF24)
4. ได้ไฟล์ TXT สะอาด

### **วิธีที่ 3: Calibre + DeDRM Plugin (แนะนำสุด)**

**Calibre คือ:**
- โปรแกรมฟรีจัดการ e-book
- อ่าน convert จัดเก็บหนังสืออิเล็กทรอนิกส์
- รองรับหลายรูปแบบ (PDF, EPUB, MOBI, AZW)

**DeDRM Plugin คือ:**
- ส่วนเสริม (plugin) สำหรับ Calibre
- ปลดล็อค DRM ออกจาก e-book
- ทำให้ไฟล์ที่ล็อคอยู่กลายเป็นไฟล์ธรรมดา

**วิธีติดตั้ง:**
1. โหลด Calibre จาก `calibre-ebook.com`
2. โหลด DeDRM plugin จาก GitHub (ชื่อ "DeDRM_tools")
3. ติดตั้ง plugin เข้า Calibre
4. ลาก e-book เข้าโปรแกรม
5. Convert → Output format เลือก TXT

**ผลลัพธ์:**
- ได้ไฟล์ TXT สะอาด ไม่มีล็อค
- แปลงได้หลายไฟล์พร้อมกัน
- ใช้เวลาไฟล์ละ 1-2 นาที

### **วิธีที่ 4: Screenshot + Google Lens**
1. Screenshot หลายหน้า
2. เปิด Google Lens → Select text → Copy
3. Paste ลง notepad → save เป็น .txt

---

## 📦 Package ที่ต้องใช้
สคริปต์ทั้งหมดใช้ package มาตรฐานที่ติดตั้งได้ด้วย:
```bash
pip install pdfplumber youtube-transcript-api pandas numpy matplotlib requests
```

---

## 🚀 เริ่มใช้งาน
1. คลิก **"Open In Colab"** ข้างบน
2. ทำตามคำแนะนำในแต่ละ notebook
3. ไม่ต้องติดตั้งอะไรในเครื่อง - ทำงานบนเบราว์เซอร์!

---

## 📋 กรณีการใช้งาน
- **งานวิจัย**: แปลงเอกสาร/หนังสือเป็นข้อความเพื่อวิเคราะห์
- **สร้างคอนเทนต์**: ดึงคำบรรยายวิดีโอมาเขียนบล็อก
- **Data Science**: สร้างชุดข้อมูลข้อความสำหรับ NLP
- **เรียนภาษา**: ดึงคำบรรยายมาเป็นสื่อการเรียน
- **สร้าง Corpus**: รวบรวมข้อความจากหลายแหล่งเพื่อฝึก AI

---

## 💡 เคล็ดลับการใช้งาน
- **PDF มี DRM**: ใช้ Calibre + DeDRM ได้ผลดีที่สุด
- **ไฟล์ใหญ่**: แบ่งเป็นส่วนย่อยก่อนแปลง
- **ภาษาไทย**: ตรวจสอบ encoding เป็น UTF-8 เสมอ
- **ข้อความเละ**: ลองเปลี่ยน PDF reader หรือใช้ OCR

---

## 🤝 ร่วมพัฒนา
ยินดีต้อนรับ:
- เพิ่มสคริปต์ใหม่ที่เป็นประโยชน์
- ปรับปรุงโค้ดที่มี
- รายงานบัคหรือแนะนำฟีเจอร์
- แชร์กรณีการใช้งาน

---

## 📄 ลิขสิทธิ์
ใช้ฟรีสำหรับการศึกษาและงานส่วนตัว

---

> 💡 **เคล็ดลับ**: กด Star ไว้เพื่อติดตามอัปเดตและสคริปต์ใหม่!
