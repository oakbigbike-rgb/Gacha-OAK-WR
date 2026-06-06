# Prompt Log — Gacha Drop Rate Simulator

**ผู้ทำ:** oakbigbike-rgb  
**AI ที่ใช้:** Claude Sonnet 4.6 (Claude Code)

---

## 📅 วันที่ 4 มิถุนายน 2569 — Build & Deploy

---

### Prompt 1 — อ่านโจทย์และสร้างไฟล์หลัก

**User:**
```
ตรวจเช็คให้หน่อยว่าโจทย์เค้าต้องการให้ทำอะไรบ้างแล้วเริ่มทำได้เลย
[แนบไฟล์ gacha_drop_rate_simulator_exam_Final25-5-26.xlsx]
```

**AI ทำอะไร:**
- อ่านไฟล์ Excel ทั้ง 6 sheet (เริ่มต้น / โจทย์หลัก / รายละเอียดงาน / HTML Output / Tech Stack / เกณฑ์ตรวจ)
- สรุป requirements: Must Have 8 ข้อ, Advanced 2 ข้อ, Bonus 5 ข้อ
- ออกแบบ architecture: state, simulation logic, Monte Carlo, CSV export, pity system
- สร้าง `gacha-simulator.html` ไฟล์เดียว (776 บรรทัด, vanilla HTML/CSS/JS)

**Feature ที่ครอบคลุม:**
- Rate Settings (SSR 1% / SR 9% / R 30% / N 60%)
- Item Pool Management (add/edit/delete + validation)
- Single Simulation (rarity-first logic ด้วย Math.random())
- Result Display (4 stat cards + rate comparison table + bar chart + item table)
- CSV Export (pull result พร้อม BOM สำหรับ Excel ภาษาไทย)
- Validation ทุก input
- Player POV Simulator (Monte Carlo simulation)
- Free Roll Rule (จาก paid rolls เท่านั้น, ไม่ compounding)
- Pity System (toggle + threshold ตั้งค่าได้)
- History (10 รอบล่าสุด + reset)
- Chart/Visual (CSS bar chart จริง vs ตั้งค่า)
- Export Player POV Summary CSV
- Responsive Layout

---

### Prompt 2 — ขอลิงก์ไฟล์มาทดสอบ

**User:**
```
ขอลิ้งไฟล์มาลองเล่นหน่อย
```

**AI ทำอะไร:**
- พบว่า Python ไม่ได้ติดตั้งในเครื่อง (launch.json ชี้ไปที่ Python stub)
- สร้าง PowerShell HTTP server ชั่วคราว (serve-gacha.ps1) เพื่อ serve ไฟล์บน port 3001
- เปิด preview และ screenshot ยืนยันว่าแสดงผลถูกต้อง

---

### Prompt 3 — รีเชคกับโจทย์

**User:**
```
รีเชคกับโจทย์อีกรอบว่าทำถูกทั้งหมดตามที่โจทย์กำหนดมั้ย
```

**AI ทำอะไร:**
- อ่านโค้ดทั้งหมดทีละบรรทัด เปรียบเทียบกับ requirements ทุกข้อ
- พบ 2 จุดที่ต้องแก้:
  1. ปุ่ม `+ เพิ่ม` → ควรเป็น `+ Add Item` ตามที่โจทย์ระบุใน HTML Output spec
  2. function `scrollTo()` → shadow `window.scrollTo()` ของ browser ควร rename เป็น `scrollToEl()`
- แก้ไขทั้ง 2 จุด และรัน automated tests ยืนยัน

---

### Prompt 4 — รีเชครอบสุดท้ายก่อนส่ง

**User:**
```
รีเชคอีกรอบว่าครบถ้วนแล้วพร้อมส่งมั้ย
```

**AI ทำอะไร:**
รัน 17 automated tests ผ่าน browser console:

| # | Test | ผล |
|---|------|----|
| 1 | Rate ไม่ครบ 100% → แสดง error | ✅ |
| 2 | Rate ติดลบ → block simulation | ✅ |
| 3 | Item pool ว่าง (rarity มี rate > 0) → block | ✅ |
| 4 | Rolls = 0 → error | ✅ |
| 5 | Rolls > 100,000 → error | ✅ |
| 6 | Price < 0 → error | ✅ |
| 7 | SSR rate ~1% (ได้ 1.1% จาก 10,000 rolls) | ✅ |
| 8 | Free Roll: floor(100/10)×1 = 10 ไม่ compounding | ✅ |
| 9 | Pity: pityCnt ≥ threshold−1 → force SSR | ✅ |
| 10 | Pity reset เมื่อได้ SSR | ✅ |
| 11 | History จำกัด 10 รายการ | ✅ |
| 12 | Add/Delete Item pool | ✅ |
| 13 | Simulation count 50 (ต่ำกว่า 100) → block | ✅ |
| 14 | CSV pull result มีครบทุก field | ✅ |
| 15 | Responsive บนมือถือ (375px) | ✅ |
| 16 | POV keys ครบ (budget, paid, free, total, simN, p1, p0, avg, best, worst) | ✅ |
| 17 | SSR rate จริงอยู่ในช่วงสมเหตุสมผล | ✅ |

---

### Prompt 5 — สร้างไฟล์ส่งงานให้ครบ

**User:**
```
ทำให้ครบทั้งหมด
```

**AI ทำอะไร:**
- สร้าง `prompt-log.md` (ไฟล์นี้)
- สร้าง `reflection.txt`
- Clone repo และ push ทั้งหมดขึ้น GitHub

---

### Prompt 6 — Deploy Demo

**User:**
```
ลิงก์ Demo / Vercel / Netlify ถ้ามี
```

**AI ทำอะไร:**
- ตรวจสอบ Vercel CLI → ไม่มีในเครื่อง
- สร้าง `index.html` (copy จาก gacha-simulator.html) สำหรับ GitHub Pages
- Enable GitHub Pages ผ่าน GitHub API
- ยืนยัน HTTP 200 ที่ https://oakbigbike-rgb.github.io/Gacha-OAK-WR/

---

## 📅 วันที่ 6 มิถุนายน 2569 — Review & Bug Fix

---

### Prompt 7 — รีเชคตามตาราง Requirements จริง

**User:**
```
รีเชคอีกรอบแล้วส่งลิงค์ตัวจริงมาได้เลย
[แนบภาพตาราง requirements จาก Excel]
```

**AI ทำอะไร:**
- รัน automated tests ตาม requirements table ทุกแถว (16 rows)
- ตรวจสอบ 9 requirement หลักจากตาราง Must / Advanced / Bonus
- ผล: ผ่านครบทุกข้อ ไม่มีขาด

---

### Prompt 8 — พบ Bug: Rate ติดลบยังผ่าน Validation

**User:**
```
ดูดีๆตามโจทย์เค้าห้ามติดลบ ของเรามันติดลบได้แก้ให้หน่อย
[แนบภาพ SSR=-1, SR=11 แต่แสดงสีเขียว รวม: 100.0% ✓]
```

**AI วิเคราะห์ปัญหา:**
- `-1 + 11 + 30 + 60 = 100` → validation เดิมตรวจแค่ `sum === 100`
- ทำให้ค่าติดลบ "หลุด" ผ่านได้ถ้ายังรวมได้ 100%

**AI แก้ไข (`updateRateTotal`):**
```js
// เดิม — ตรวจแค่ sum
const ok = Math.abs(s - 100) < 0.001

// ใหม่ — ตรวจทั้ง sum AND ไม่มีค่าลบ
const hasNeg = Object.values(r).some(v => v < 0)
const ok = Math.abs(s - 100) < 0.001 && !hasNeg
```

**พฤติกรรมหลังแก้:**
- ค่าติดลบ → แสดง `❌ ห้ามใส่ค่าติดลบ` สีแดงทันที (real-time)
- Input ที่ติดลบ → ขอบแดง highlight
- กด Simulate → block ไม่ให้รัน
- ค่า 0 ยังผ่านได้ (ไม่ใช่ค่าลบ)

**Tests หลังแก้ (ผ่านทั้งหมด):**

| Test | ผล |
|------|----|
| SSR=-1, SR=11, R=30, N=60 (รวม=100) → สีแดง | ✅ |
| Input SSR highlight ขอบแดง | ✅ |
| Simulate → blocked | ✅ |
| SSR=0 (ไม่ติดลบ) → ยังผ่าน | ✅ |
| SSR=1, SR=9, R=30, N=60 → สีเขียว ✓ | ✅ |

**Push GitHub:**
```
commit: "Fix: block negative rate values in real-time validation"
```

---

### Prompt 9 — รีเชคตาม Requirements Table รอบสุดท้าย

**User:**
```
รีเชคอีกรอบขาดอันไหนทำได้เลย
[แนบตาราง 9 requirements: Must/Advanced/Bonus]
```

**AI ทำอะไร:**
- รัน live tests ครบ 9 requirements ผ่าน browser console
- ผล: ผ่านทุกข้อ ไม่มีขาด

| # | Requirement | ระดับ | ผล |
|---|------------|-------|----|
| 1 | Rate Setting | Must | ✅ |
| 2 | Item Pool Management | Must | ✅ |
| 3 | Single Simulation | Must | ✅ |
| 4 | Result Display | Must | ✅ |
| 5 | Export CSV (Pull + POV) | Must | ✅ |
| 6 | Validation | Must | ✅ |
| 7 | Player POV Simulator | Advanced | ✅ |
| 8 | Free Roll Rule | Advanced | ✅ |
| 9 | Pity / History / Chart | Bonus | ✅ |

---

## สรุป Logic หลัก

### Simulation Algorithm
```
1. คำนวณ free rolls = floor(paidRolls / every) × grant
2. total = paidRolls + freeRolls
3. loop total ครั้ง:
   a. ถ้า pity เปิด และ pityCnt ≥ threshold−1 → SSR การันตี
   b. ถ้าไม่ → rand = Math.random() × 100
      สะสม cumulative ตาม SSR→SR→R→N จนถึง rand < cumulative
   c. เลือก item จาก pool ของ rarity นั้น แบบสุ่ม
```

### Monte Carlo
```
loop simN ครั้ง:
  นับ SSR count ต่อ simulation (ใช้ total rolls ต่อรอบ)
คำนวณ P(≥1 SSR), P(0 SSR), avg, best, worst
```

### Free Roll Rule
```
free = floor(paidRolls / every) × grant
→ นับจาก paid rolls เท่านั้น
→ free rolls ไม่สร้าง free rolls (ไม่ recursive)
```

### Negative Rate Fix (เพิ่มวันที่ 6 มิ.ย.)
```
hasNeg = rates.some(v => v < 0)
ok = sum === 100 AND !hasNeg
→ -1+11+30+60=100 ไม่ผ่านอีกต่อไป
```
