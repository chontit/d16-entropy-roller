# 🚀 Release: D16 · Entropy Roller — v1.0.1

**Patch release / รีลีสแก้ไขย่อย**
Refines shake-to-roll so only a firm, deliberate shake rolls the die, and adds an on/off toggle for it.
ปรับ shake-to-roll ให้ต้องเขย่าแรงและตั้งใจจริงถึงจะทอย พร้อมเพิ่มปุ่มเปิด/ปิด

---

> ## ⚠️ CRITICAL SCOPE / ขอบเขตการใช้งาน
>
> **EN:** This tool is strictly an **educational simulator**. **Do NOT** use its output to generate real Bitcoin seeds, private keys, or anything holding financial value. See [SECURITY.md](../SECURITY.md) for the full threat model.
>
> **TH:** เครื่องมือนี้เป็นเพียง **Educational Simulator (สื่อการเรียนรู้)** เท่านั้น **ห้าม** นำผลลัพธ์ที่ได้ไปสร้าง Seed Phrase หรือกุญแจส่วนตัว (Private Key) สำหรับเก็บสินทรัพย์จริงเด็ดขาด กรุณาอ่าน [SECURITY.md](../SECURITY.md) สำหรับขอบเขตความปลอดภัย

---

## ✨ What's new in v1.0.1 / มีอะไรใหม่

*   🎚️ **Higher shake threshold / เพิ่มเกณฑ์การเขย่า:** the shake-to-roll trigger was raised (14 → 25) so light movement, bumps, or setting the phone down no longer roll the die by accident.
    เกณฑ์ trigger ถูกเพิ่ม (14 → 25) การขยับเบา ๆ, กระแทกเล็กน้อย หรือวางเครื่องลง จะไม่ทอยพลาดอีก
*   🔀 **Shake-to-roll on/off toggle / ปุ่มเปิด-ปิดการเขย่า:** a switch on the main screen (shown on motion-capable devices) turns shake-to-roll on or off. It is **independent of the Motion entropy source** — you can switch shake off while still mixing motion noise into the roll.
    สวิตช์บนหน้าหลัก (แสดงบนอุปกรณ์ที่มี motion) เปิด/ปิดการเขย่าได้ **แยกอิสระจาก Motion entropy source** — ปิดการเขย่าได้โดยยังใช้ motion noise ในการมิกซ์ต่อ
*   🗺️ **In-tool pipeline flowchart / แผนผังระบบในตัว tool:** the *How it works* section now shows a diagram of the entropy flow — sources → XOR pool → SHA-256 → D16 roll.
    หัวข้อ *How it works* เพิ่มแผนผังแสดงการไหลของ entropy — sources → XOR pool → SHA-256 → ผล D16

> The entropy pipeline, SHA-256 extractor, and security model are **unchanged** from v1.0.0.
> pipeline entropy, SHA-256 extractor และโมเดลความปลอดภัย **ไม่เปลี่ยน** จาก v1.0.0
>
> Full feature list & docs: [README.md](../README.md) · [CHANGELOG.md](../CHANGELOG.md)

---

## 📦 Assets in this release / ไฟล์ที่แนบมาในรีลีส

| File / ชื่อไฟล์ | Purpose / วัตถุประสงค์ |
| :--- | :--- |
| `d16-entropy-roller.html` | ตัวแอปพลิเคชัน (สามารถเปิดใช้งานโดยตรงบนเว็บเบราว์เซอร์ได้ทันที) |
| `d16-entropy-roller.html.asc` | ลายเซ็นดิจิทัล PGP (Detached signature) สำหรับตรวจสอบไฟล์ HTML |
| `SHA256SUMS` | ไฟล์รวมค่า Checksum ของไฟล์ทั้งหมด |
| `SHA256SUMS.asc` | ลายเซ็นดิจิทัล PGP สำหรับตรวจสอบไฟล์ Checksum |

---

## 🔐 Verify before use / ตรวจสอบความปลอดภัยก่อนใช้งาน

เราแนะนำให้คุณตรวจสอบความถูกต้องของไฟล์ทุกครั้งก่อนเปิดใช้งาน:

```bash
# 1. นำเข้าและยืนยัน Signing key ของผู้พัฒนา
gpg --import chollatis-bitcoiner-pubkey.asc
gpg --fingerprint chon_tit@hotmail.com

# * คาดหวังผลลัพธ์ (Expected Fingerprint):
# EEFC F3F0 928D 0199 BA7E  56EC 2DB5 4085 AB23 3A47

# 2. ตรวจสอบลายเซ็นของตัวแอปพลิเคชัน
gpg --verify d16-entropy-roller.html.asc d16-entropy-roller.html

# 3. (ทางเลือก) ตรวจสอบค่า Checksums
gpg --verify SHA256SUMS.asc SHA256SUMS
sha256sum -c SHA256SUMS
```

---

## 🚀 How to run / วิธีเปิดใช้งาน

เพียงแค่ **ดับเบิลคลิก** ที่ไฟล์ `d16-entropy-roller.html` เพื่อเปิดผ่านเบราว์เซอร์
เพื่อความปลอดภัยสูงสุด แนะนำให้คัดลอกไฟล์นี้ไปเปิดบนเครื่องที่ตัดขาดจากอินเทอร์เน็ต (Air-gapped machine) เช่น ระบบปฏิบัติการ Tails ไม่จำเป็นต้องรันเซิร์ฟเวอร์, ไม่ต้อง Build และไม่ต้องใช้อินเทอร์เน็ต

---

## 📌 Platform Notes / หมายเหตุเพิ่มเติม

* **Tor Browser / Tails:** เบราว์เซอร์เหล่านี้มักจะบล็อกการเข้าถึงกล้องและไมโครโฟนเพื่อความเป็นส่วนตัว ซึ่งจะทำให้แหล่ง Source เหล่านั้นแสดงสถานะเป็น `DENIED` ถือเป็นพฤติกรรมปกติ (Expected) โดยระบบจะยังคงทำงานได้ผ่าน Baseline ของ CSPRNG
* **iOS (Apple):** ระบบอาจต้องการการอนุญาตเพิ่มเติมสำหรับ Motion Sensor ผู้ใช้ต้องแตะปุ่ม *enable* หรือเปิด toggle *เขย่าเพื่อทอย* เพื่อให้ระบบทำงานได้

---

*"Don't Trust, Verify." — Chollatis Bitcoiner*
