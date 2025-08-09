# Full Reasoning Prompt Generator (45 Types)

ตัวสร้าง Reasoning Prompts คุณภาพแบบ “ทำมือ” ครอบคลุม reasoning type 9 umbrella / 45 subtypes  
ใช้งานได้ทั้ง **Google Colab**, **VS Code**, และเครื่อง Local ของคุณ  
สร้างไฟล์ `.md` พร้อมตาราง **Redundancy Check** ต่อท้ายเพื่อดูความซ้ำของ `(domain_context, sub_type, fallacy)`

---

## ฟีเจอร์เด่น

- ครอบคลุม 9 umbrella reasoning types / 45 subtypes
- ทำงานออฟไลน์ 100% ไม่ต้องใช้ API
- โครงสร้างผลลัพธ์ตาม MASTER_FULL (TH/EN + ช่อง ZH guard)
- มีตัวตรวจ Redundancy อัตโนมัติ
- เลือกบันทึกไฟล์ลงเครื่อง (`/content`) หรือ Google Drive ได้
- ปรับ `start_num`, `n`, `reasoning_type`, `seed` และ `save_path` ได้ตามต้องการ

---

## โครงสร้าง reasoning_type ที่รองรับ

`analogical`, `causal`, `probabilistic`, `counterfactual`, `temporal`, `abductive`, `meta_reasoning`, `autonomous_cognitive_system`, `emergent_intelligence`

---

## การใช้งานบน Google Colab

### 1. เปิด Notebook ใหม่

ไปที่ [Google Colab](https://colab.research.google.com/) → New Notebook

### 2. Cell แรก — วางโค้ดยาวตัว Generator

- วางโค้ดจากไฟล์ `full_reasoning_prompt_generator_45types_v1.py` ทั้งหมดใน cell แรก
- กด **▶️ Run**
- ตรวจว่าโหลดสำเร็จ: พิมพ์ `gen_batch` แล้วกด **Shift+Tab** ดูว่ามีฟังก์ชันขึ้นมา

### 3. Cell ต่อไป — เลือกวิธีบันทึก

#### 3.1 บันทึกลงเครื่อง Colab (`/content`)

```python
# === RUN LOCAL (บันทึกลงเครื่อง Colab ที่ /content) ===
START_NUM     = 287
N_PROMPTS     = 14
REASONING_TYP = "analogical"  # หรือเลือก reasoning type อื่น
SEED          = 2025
SAVE_PATH     = "/content/prompts_287_300.md"

result = gen_batch(start_num=START_NUM, n=N_PROMPTS, reasoning_type=REASONING_TYP,
                   seed=SEED, save_path=SAVE_PATH)
print(result[:1000] + "\n...")
print("ไฟล์ถูกสร้างแล้วที่:", SAVE_PATH)
ไฟล์จะอยู่ในแถบ Files ของ Colab → /content/prompts_287_300.md

3.2 บันทึกลง Google Drive
Mount Google Drive (ครั้งแรกของ session)

python
คัดลอก
แก้ไข
from google.colab import drive
drive.mount('/content/drive')
สร้างไฟล์ใน Drive

python
คัดลอก
แก้ไข
START_NUM     = 301
N_PROMPTS     = 10
REASONING_TYP = "causal"
SEED          = 123
SAVE_PATH     = "/content/drive/MyDrive/Reasoning/prompts_causal_301_310.md"

result = gen_batch(start_num=START_NUM, n=N_PROMPTS, reasoning_type=REASONING_TYP,
                   seed=SEED, save_path=SAVE_PATH)
print("Saved to:", SAVE_PATH)
3.3 (ไม่บังคับ) ดึงเฉพาะ Redundancy Check เป็น .csv
python
คัดลอก
แก้ไข
import re, csv

md_path  = SAVE_PATH
csv_path = SAVE_PATH.replace(".md", "_redundancy.csv")

with open(md_path, "r", encoding="utf-8") as f:
    text = f.read()

rows = []
capture = False
for line in text.splitlines():
    if line.strip().startswith("| Prompt |"):
        capture = True
        continue
    if capture:
        if not line.strip().startswith("|"):
            break
        cols = [c.strip() for c in line.strip().strip("|").split("|")]
        if cols and cols[0] != '---:':
            rows.append(cols)

with open(csv_path, "w", newline="", encoding="utf-8") as f:
    writer = csv.writer(f)
    writer.writerow(["Prompt","domain_context","sub_type","fallacy"])
    for r in rows:
        writer.writerow(r)

print("Redundancy CSV ->", csv_path)
การใช้งานบน VS Code / Local
ติดตั้ง Python 3.9+

สร้างไฟล์ full_reasoning_prompt_generator_45types_v1.py และวางโค้ด generator ทั้งหมด

สร้างไฟล์ run_generator.py เพื่อเรียกใช้งาน เช่น:

python
คัดลอก
แก้ไข
from full_reasoning_prompt_generator_45types_v1 import gen_batch

result = gen_batch(start_num=1, n=5, reasoning_type="analogical",
                   seed=42, save_path="prompts_1_5.md")
print(result)
รันใน Terminal:

bash
คัดลอก
แก้ไข
python run_generator.py
พารามิเตอร์หลัก
start_num: หมายเลข prompt เริ่มต้น

n: จำนวน prompt ที่ต้องการสร้าง

reasoning_type: เลือกจาก 9 umbrella reasoning types

seed: ทำให้สุ่มซ้ำได้

save_path: พาธบันทึกไฟล์ปลายทาง

ผลลัพธ์
ไฟล์ .md จะมี:

Prompt เต็มรูปแบบใน code block

ตาราง Redundancy Check อยู่นอก code block เพื่อบอกว่ามี (domain_context, sub_type, fallacy) ซ้ำกันหรือไม่

Troubleshooting
NameError: gen_batch not defined
→ ยังไม่ได้รัน Cell แรกที่วางโค้ดยาวตัว Generator

Permission denied (Google Drive)
→ ยังไม่ได้ mount drive หรือ path ปลายทางไม่มีโฟลเดอร์

รันแล้วไม่มีอะไรเกิดขึ้น
→ ตรวจว่าบรรทัดเรียก gen_batch(...) ไม่มี # อยู่ข้างหน้า (ไม่ถูกคอมเมนต์)

yaml
คัดลอก
แก้ไข

---
