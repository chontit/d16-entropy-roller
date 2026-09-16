# 🔒 Security Policy / นโยบายความปลอดภัย

> ⚠️ **Educational simulator only — never use it to generate real seeds or keys.**
> **เครื่องมือเพื่อการเรียนรู้เท่านั้น — ห้ามใช้สร้าง seed / กุญแจจริง**

<p align="center">
  <strong><a href="#english">🇬🇧 English</a> &nbsp;|&nbsp; <a href="#thai">🇹🇭 ภาษาไทย</a></strong>
</p>

---

<a id="english"></a>

# 🇬🇧 English

### 1. Scope in one sentence

D16 Entropy Roller is an **educational simulator** for visualizing dice-based and sensor-based randomness. It is **not** a wallet, **not** a seed generator, and **must not** be used to produce entropy for any key that will hold real value.

### 2. Threat model

**What the tool DOES give you**

- **Statistically uniform output** on each roll (SHA-256 extractor, unbiased `& 0x0F`).
- **Defense-in-depth mixing** — the "1-of-N independent source" property: if any one input is genuinely random and independent, the combined output stays uniform.
- **Transparency** — live indicators show which sources are active and how much entropy each contributes.
- **A raised floor against *unintentional* CSPRNG bugs** by folding in independent physical noise.

**What the tool does NOT protect against**

- **A compromised host (deep backdoor).** If the OS/driver/firmware is malicious, it can feed fabricated "sensor" data, suppress your real input, or bias `crypto.getRandomValues()` while every statistical test still passes. Software running on that host cannot bootstrap trust it does not have.
- **Provenance verification.** No indicator can prove a stream is *physically* sourced rather than a well-behaved PRNG spoof.

**The core reason**

Software randomness is **unverifiable by construction**. You can observe the *output*, but you cannot verify the *process* with your own senses. A physical die is different: you watch it land. That first-person verifiability — not statistical quality — is why physical dice remain the trust anchor for real custody. *(Reference: the July 2026 Coldcard RNG firmware incident, in which an audited dedicated TRNG failed silently — good statistical output is not proof of a healthy source.)*

### 3. If you need real entropy

Use **physical dice** (a D16 gives 4 clean bits per roll; 64 rolls = 256 bits) processed by a **verifiable, offline extractor** that guarantees uniform output even from a biased die, ideally on an air-gapped, amnesiac OS (e.g. Tails), with a second independent implementation to cross-verify the resulting mnemonic. Consider a mixing design where physical dice are the mandatory base and any software RNG is only an XOR supplement — never the originator.

### 4. Reporting a vulnerability

If you find a correctness or safety issue (e.g. a biased extraction path, a source that misreports its state, a hidden network request), please report it privately first:

- Open a **[GitHub Security Advisory](../../security/advisories/new)** (preferred), **or**
- Email the maintainer using the PGP key in [`chollatis-bitcoiner-pubkey.asc`](chollatis-bitcoiner-pubkey.asc), encrypting your report.

Please include: browser & OS, steps to reproduce, and expected vs actual behavior. Allow a reasonable window for a fix before public disclosure.

### 5. Supported versions

| Version | Supported |
|---|---|
| 1.0.x | ✅ |
| < 1.0 | ❌ |

### 6. Signature verification

Every release is signed. Verify before use — see the **Verify authenticity — PGP** section in [README.md](README.md#english).

Fingerprint: `EEFC F3F0 928D 0199 BA7E 56EC 2DB5 4085 AB23 3A47`

<p align="right"><sub><a href="#english">⤴ top</a> · <a href="#thai">🇹🇭 อ่านภาษาไทย</a></sub></p>

---

<a id="thai"></a>

# 🇹🇭 ภาษาไทย

### 1. ขอบเขตในหนึ่งประโยค

D16 Entropy Roller เป็น **educational simulator** สำหรับสาธิตการสุ่มจากลูกเต๋าและ sensor **ไม่ใช่** wallet **ไม่ใช่** เครื่องสร้าง seed และ **ห้าม** ใช้สร้าง entropy สำหรับกุญแจใด ๆ ที่ถือเงินจริง

### 2. โมเดลภัยคุกคาม

**สิ่งที่ tool ให้**

- **output สม่ำเสมอทางสถิติ** ทุกทอย (SHA-256 extractor, unbiased `& 0x0F`)
- **การผสมแบบ defense-in-depth** — คุณสมบัติ "1 ใน N อิสระ": ถ้ามีอย่างน้อย 1 input ที่สุ่มจริงและอิสระ ผลรวมยัง uniform
- **ความโปร่งใส** — indicator แสดง real-time ว่าแหล่งไหน active และป้อน entropy เท่าไร
- **ยกพื้นกัน bug ของ CSPRNG แบบ *ไม่ตั้งใจ*** ด้วยการผสม physical noise อิสระเข้าไป

**สิ่งที่ tool ไม่ป้องกัน**

- **host ที่ถูก compromise (deep backdoor)** — ถ้า OS/driver/firmware ร้าย มันป้อน sensor data ปลอม, ทิ้ง input จริงของคุณ, หรือทำให้ `getRandomValues()` เอียง โดย output ยังผ่านทุก test ได้ software บน host นั้น bootstrap ความเชื่อที่ตัวเองไม่มีขึ้นมาไม่ได้
- **การพิสูจน์ที่มา** — ไม่มี indicator ใดพิสูจน์ได้ว่า stream มาจาก physical จริง ไม่ใช่ PRNG ปลอมที่เนียน

**เหตุผลแกนกลาง**

การสุ่มด้วย software **verify ไม่ได้โดยโครงสร้าง** คุณเห็น *output* แต่ verify *กระบวนการ* ด้วยประสาทสัมผัสตัวเองไม่ได้ ลูกเต๋าจริงต่างออกไป — คุณเห็นมันตก first-person verifiability นี้ (ไม่ใช่คุณภาพทางสถิติ) คือเหตุผลที่ลูกเต๋าจริงยังเป็น trust anchor ของ custody จริง *(อ้างอิง: เคส Coldcard RNG ก.ค. 2026 ที่ TRNG เฉพาะทางซึ่ง audit แล้วยังพังเงียบ — output ที่ดูสุ่มดีไม่ใช่หลักฐานว่า source แข็งแรง)*

### 3. ถ้าต้องการ entropy จริง

ใช้ **ลูกเต๋าจริง** (d16 ให้ 4 บิตสะอาดต่อครั้ง; 64 ครั้ง = 256 บิต) ประมวลผลด้วย **extractor ที่ตรวจสอบได้และออฟไลน์** ซึ่งรับประกัน uniform แม้ลูกเต๋าเอียง ทำบน OS แบบ air-gapped/amnesiac (เช่น Tails) และ cross-verify mnemonic ด้วย implementation ที่ 2 ที่อิสระกัน ออกแบบให้ **ลูกเต๋าจริงเป็น base บังคับ** และ software RNG เป็นแค่ XOR เสริม — ไม่ใช่ originator

### 4. รายงานช่องโหว่

ถ้าพบปัญหาด้านความถูกต้อง/ความปลอดภัย (เช่น extraction ที่เอียง, source ที่รายงานสถานะผิด, network request ซ่อนเร้น) โปรดรายงานแบบส่วนตัวก่อน:

- เปิด **[GitHub Security Advisory](../../security/advisories/new)** (แนะนำ) **หรือ**
- อีเมลถึงผู้ดูแลโดยเข้ารหัสด้วย PGP key ใน [`chollatis-bitcoiner-pubkey.asc`](chollatis-bitcoiner-pubkey.asc)

โปรดแนบ: browser & OS, ขั้นตอน reproduce, และพฤติกรรมที่คาดหวัง vs ที่เกิดจริง และเผื่อเวลาสมควรให้แก้ไขก่อนเปิดเผยสาธารณะ

### 5. เวอร์ชันที่ดูแล

| Version | Supported |
|---|---|
| 1.0.x | ✅ |
| < 1.0 | ❌ |

### 6. การตรวจสอบลายเซ็น

ทุก release เซ็นลายเซ็น ตรวจสอบก่อนใช้ — ดูหัวข้อ **ตรวจสอบลายเซ็น — PGP** ใน [README.md](README.md#thai)

Fingerprint: `EEFC F3F0 928D 0199 BA7E 56EC 2DB5 4085 AB23 3A47`

<p align="right"><sub><a href="#thai">⤴ บนสุด</a> · <a href="#english">🇬🇧 Read in English</a></sub></p>

---

<p align="center">
  <sub>Built by <strong>Chollatis Maneewong</strong> · Chollatis Bitcoiner · <a href="https://learning.chontit.win">learning.chontit.win</a></sub><br>
  <sub><em>"Don't Trust, Verify."</em></sub>
</p>
