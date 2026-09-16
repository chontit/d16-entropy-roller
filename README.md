<h1 align="center">🎲 D16 · Entropy Roller</h1>

<p align="center">
  <em>A stylish, fully-offline 16-sided dice simulator with transparent, sensor-fused randomness.</em><br>
  <em>เครื่องจำลองการทอยลูกเต๋า 16 หน้า แบบออฟไลน์ 100% พร้อมแสดงแหล่ง noise ของอุปกรณ์อย่างโปร่งใส</em>
</p>

<p align="center">
  <img alt="license" src="https://img.shields.io/badge/license-MIT-f4b13a">
  <img alt="offline" src="https://img.shields.io/badge/network-100%25%20offline-34e2d0">
  <img alt="single-file" src="https://img.shields.io/badge/build-single--file%20HTML-34e2d0">
  <img alt="dependencies" src="https://img.shields.io/badge/dependencies-0-34e2d0">
  <img alt="pgp" src="https://img.shields.io/badge/releases-PGP%20signed-f4b13a">
  <img alt="version" src="https://img.shields.io/badge/version-1.0.0-blue">
</p>

<p align="center"><strong>"Don't Trust, Verify."</strong> — Chollatis Bitcoiner</p>

---

> ⚠️ **Educational simulator only — do NOT use its output for real Bitcoin seeds or keys.**
> **เครื่องมือเพื่อการเรียนรู้เท่านั้น — ห้ามนำผลลัพธ์ไปสร้าง seed / กุญแจจริง**

<p align="center">
  <strong><a href="#english">🇬🇧 English</a> &nbsp;|&nbsp; <a href="#thai">🇹🇭 ภาษาไทย</a></strong>
</p>

---

<a id="english"></a>

# 🇬🇧 English

### ⚠️ Read this first

This is a **simulator and educational tool**. It demonstrates what dice-based entropy looks like and how a browser can harvest physical noise from device sensors. **Do NOT use its output to generate a Bitcoin seed, private key, or anything that will hold real value.** Software randomness — even a good CSPRNG mixed with sensor noise — is *unverifiable by design* against a compromised OS/driver/firmware. For real custody, the trust anchor must be **physical dice you watch land with your own eyes**, processed by a verifiable extractor.

👉 For real seed generation, use physical dice with a verifiable tool such as a dice-roll extractor or [Coldcard](https://coldcard.com) dice-roll mode. See **[SECURITY.md](SECURITY.md)** for the full threat model.

### What it is

A single `.html` file. Open it, click the die (or *shake* on mobile), and it lands on a value **0–F** (a 16-sided roll). Every result is distilled from several independent noise sources — the system CSPRNG plus, where available, your camera, microphone, and motion sensors — combined through a SHA-256 extractor. A collapsible panel shows exactly **which sources are active** and how much real entropy each is contributing, in real time.

There is **no seed step, no wallet step, no export** — dice rolling only, by design.

### Features

|   | Feature | Description |
|---|---|---|
| 🎲 | **D16 roller with tumble animation** | 16-sided roll with a faceted-gem HUD animation |
| 🎛️ | **Quick-select entropy chips** | Tap CSPRNG / Cam / Mic / Motion on the main screen to choose which sources feed the mix — no menu needed |
| 📱 | **Shake-to-roll on mobile** | Shake the device to roll (accelerometer) |
| 🛰️ | **Sensor-fused entropy** | Mixes only the selected sources: CSPRNG ⊕ camera ⊕ mic ⊕ motion, via SHA-256 |
| 🔬 | **Live source panel** | See each source's state (`ACTIVE / DENIED / OFF / N/A`) and `IN MIX` in real time |
| 📊 | **Health indicators** | min-entropy (bits/byte), RCT, APT per source |
| 🤝 | **Perturbation self-test** | Cover the lens / shake / make a sound and watch the source respond |
| 🌐 | **100% offline** | No network; no external CDN / font / analytics |
| 📦 | **Single file, zero dependencies** | One file, no build, no dependencies |
| 🌓 | **Dark HUD / cyberpunk UI** | Bilingual TH/EN |
| ♿ | **Accessible** | Keyboard-operable, reduced-motion, responsive |

### Quick start

```bash
# 1) Download the single file (or clone the repo)
git clone https://github.com/<your-username>/d16-entropy-roller.git

# 2) (recommended) Verify the PGP signature — see the section below
gpg --verify d16-entropy-roller.html.asc d16-entropy-roller.html

# 3) Open it — no server, no build
#    double-click the file, or:
xdg-open d16-entropy-roller.html      # Linux
open d16-entropy-roller.html          # macOS
```

For maximum isolation, copy the file to an air-gapped machine (e.g. Tails OS) and open it there. No internet connection is ever required or used.

### How it works

On each roll, the tool builds a message from the **selected-source accumulator** XOR-ed with a fresh CSPRNG draw (when CSPRNG is selected), plus a monotonic counter and a high-resolution timestamp. It hashes that with SHA-256 and takes the **low nibble** of the first output byte:

```text
material = selected_pool ⊕ csprng(32 bytes) ‖ counter(4) ‖ hi-res-time(8)
digest   = SHA-256(material)
value    = digest[0] & 0x0F        // 0..15, no modulo bias (256 ÷ 16 is exact)
pool    ⊕= digest                  // hash-chain: next roll differs even with no sensors
```

**Why `& 0x0F` is unbiased:** a uniform byte spans 0–255; since 256 is an exact multiple of 16, its low 4 bits are perfectly uniform over 0–15. No rejection sampling needed.

**The combiner rule:** if **at least one** input source is genuinely random and independent of the others, the XOR/hash output is uniform — the weak sources cannot degrade it.

**Selected sources only:** only the sources you tick in the chips are folded into the accumulator; `input-timing` is shown for diagnostics but is never mixed.

> The visual "tumble" uses `Math.random()` for cosmetics **only**. The value the die lands on always comes from the SHA-256 extractor above.

### Entropy sources

| Source | Physical basis | API | Quality | Notes |
|---|---|---|---|---|
| **System CSPRNG** | OS entropy pool | `crypto.getRandomValues()` | baseline, always on | statistically ~100% uniform, but the process itself can't be self-verified |
| **Camera** | thermal / shot noise of the image sensor | `getUserMedia` → canvas → **frame-diff LSB** | high | denoise/auto-exposure strips noise → uses frame differences |
| **Microphone** | Johnson-Nyquist noise of the ADC | `getUserMedia` (suppression off) → `AnalyserNode` | good | disables `noiseSuppression / AGC / echoCancellation` |
| **Motion** | MEMS thermal + human movement | `DeviceMotionEvent` | good | mobile; iOS requires tapping enable |
| **Input timing** | tap / mouse jitter | pointer / key events | weak | display only · not mixed (browser clamps resolution vs Spectre) |

Camera, mic, and motion carry *true physical* entropy and are opt-in (a browser permission prompt appears when you enable each). The CSPRNG is always available as a statistical baseline.

### Health indicators

Each active source shows live diagnostics:

- **min-entropy (bits/byte)** — Most-Common-Value estimate of real randomness per byte. Closer to 8 is better.
- **RCT** *(Repetition Count Test)* — detects a **stuck / dead** source (identical values in a row beyond a cutoff).
- **APT** *(Adaptive Proportion Test)* — detects **bias** within a 512-sample window.
- **ACTIVITY** — short-term variance; used with the perturbation self-test.

> **Honest limit:** these indicators can **falsify** a source (prove it's dead/biased) but can **never verify** that a stream is truly physical rather than a well-behaved PRNG spoof — because a good CSPRNG passes every statistical test by definition. The strongest check is the **perturbation self-test**, where *you* physically disturb the sensor and watch the stream respond (the oracle is outside the machine).

### Verify authenticity — PGP

Every release is signed with the maintainer's OpenPGP key. Verify before trusting any copy you downloaded.

```bash
# 1) Import the public key (bundled as chollatis-bitcoiner-pubkey.asc)
gpg --import chollatis-bitcoiner-pubkey.asc

# 2) Confirm the fingerprint matches EXACTLY:
gpg --fingerprint chon_tit@hotmail.com
#    Expected:
#    EEFC F3F0 928D 0199 BA7E  56EC 2DB5 4085 AB23 3A47

# 3) Verify the detached signature of the tool
gpg --verify d16-entropy-roller.html.asc d16-entropy-roller.html
#    Look for: "Good signature from Chollatis Maneewong - Bitcoiner"
```

| Field | Value |
|---|---|
| Identity | `Chollatis Maneewong - Bitcoiner <chon_tit@hotmail.com>` |
| Algorithm | ed25519 (curve25519) |
| Fingerprint | `EEFC F3F0 928D 0199 BA7E 56EC 2DB5 4085 AB23 3A47` |

> A "Good signature" only confirms the file matches what the maintainer signed. It does **not** vouch for fitness for any purpose — the security scope in [SECURITY.md](SECURITY.md) still applies.

### Verify it's really offline

Don't take our word for it — verify:

1. **Read the source.** It's one human-readable file. Search for `http`, `fetch`, `XMLHttpRequest`, `import`, `<script src` — there are none pointing outside the file.
2. **Disconnect the network / pull the cable**, then use the tool. It works fully.
3. **Open DevTools → Network tab**, reload, and roll. Zero outbound requests.

### Browser support & permissions

| Feature | Chrome/Edge | Firefox | Safari (iOS) | Tor Browser / Tails |
|---|---|---|---|---|
| CSPRNG roll | ✅ | ✅ | ✅ | ✅ |
| Camera / Mic | ✅ prompt | ✅ prompt | ✅ prompt | ⚠️ usually blocked |
| Motion / shake | – | – | ✅ (tap enable) | – |

> **On Tails / Tor Browser**, camera & mic are typically blocked for privacy, so those sources will read `DENIED`. **This is expected, not a bug** — the tool still runs fully on the CSPRNG baseline. `crypto.subtle` and `getUserMedia` require a *secure context*; `file://` qualifies in modern browsers.

### FAQ

**Q: Can I use this to make a real Bitcoin seed?**
No. See the warning at the top and [SECURITY.md](SECURITY.md). Use physical dice + a verifiable extractor for real funds.

**Q: Why mix sensors if the CSPRNG is already good?**
To demonstrate defense-in-depth (the "1-of-N independent source" property) and to *show* device noise transparently. It raises the floor against unintentional CSPRNG bugs — it does not defeat a deep backdoor.

**Q: Why D16 (hex)?**
A 16-sided die maps to exactly one hex nibble (4 bits) with zero waste and zero modulo bias — the cleanest common dice format.

**Q: Does the tumble animation affect the result?**
No — it's cosmetic. The landed value always comes from SHA-256.

### Security scope

This tool is intentionally scoped as an **educational simulator**. The full trust model, what it does and does not protect against, and how to report issues are documented in **[SECURITY.md](SECURITY.md)**. Please read it before drawing any conclusions about entropy quality.

### License

Released under the **MIT License** — see [LICENSE](LICENSE).

<p align="right"><sub><a href="#english">⤴ top</a> · <a href="#thai">🇹🇭 อ่านภาษาไทย</a></sub></p>

---

<a id="thai"></a>

# 🇹🇭 ภาษาไทย

### ⚠️ อ่านก่อนใช้งาน

เครื่องมือนี้เป็น **simulator / สื่อการเรียนรู้ เท่านั้น** ไว้สาธิตว่า entropy จากลูกเต๋าหน้าตาเป็นอย่างไร และ browser ดึง physical noise จาก sensor ของอุปกรณ์ได้อย่างไร **ห้ามนำผลลัพธ์ไปสร้าง Bitcoin seed / private key หรืออะไรก็ตามที่ถือเงินจริงเด็ดขาด** เพราะการสุ่มด้วย software — ต่อให้เป็น CSPRNG ดี ๆ ผสม sensor noise — **verify กระบวนการเองไม่ได้** เมื่อ OS/driver/firmware ถูก compromise สำหรับ custody จริง trust anchor ต้องเป็น **ลูกเต๋าจริงที่คุณเห็นตกกับตา** แล้วประมวลผลด้วย extractor ที่ตรวจสอบได้

👉 สำหรับการสร้าง seed จริง ใช้ลูกเต๋าจริงคู่กับเครื่องมือที่ตรวจสอบได้ เช่น dice-roll extractor หรือโหมด dice-roll ของ [Coldcard](https://coldcard.com) ดู threat model เต็มได้ที่ **[SECURITY.md](SECURITY.md)**

### คืออะไร

ไฟล์ `.html` ไฟล์เดียว เปิดขึ้นมา แตะลูกเต๋า (หรือ *เขย่า* บนมือถือ) แล้วออกผล **0–F** (ทอย 16 หน้า) ทุกผลลัพธ์กลั่นมาจากหลายแหล่ง noise ที่อิสระต่อกัน — CSPRNG ของระบบ บวกกับ camera / microphone / motion sensor เท่าที่มี — ผสมผ่าน SHA-256 extractor มีแผงพับเก็บที่แสดง **real-time ว่าแหล่งไหน active** และแต่ละแหล่งป้อน entropy จริงแค่ไหน

**ไม่มี** ขั้นตอนสร้าง seed / wallet / export ใด ๆ — ทอยลูกเต๋าอย่างเดียวตามตั้งใจ

### ความสามารถ

|   | ฟีเจอร์ | รายละเอียด |
|---|---|---|
| 🎲 | **D16 Roller** | ทอย 16 หน้า พร้อม animation แบบ faceted-gem HUD |
| 🎛️ | **Quick-select chips** | ติ๊กเลือกแหล่งสุ่ม (CSPRNG / Cam / Mic / Motion) เข้ามิกซ์ได้จากหน้าหลัก ไม่ต้องเปิดเมนู |
| 📱 | **Shake-to-Roll** | เขย่าเครื่องเพื่อทอย (accelerometer) |
| 🛰️ | **Sensor-fused entropy** | ผสมเฉพาะแหล่งที่เลือก: CSPRNG ⊕ camera ⊕ mic ⊕ motion ผ่าน SHA-256 |
| 🔬 | **Live source panel** | รู้สถานะแต่ละแหล่ง (`ACTIVE / DENIED / OFF / N/A`) และ `IN MIX` แบบ real-time |
| 📊 | **Health indicators** | min-entropy (bits/byte), RCT, APT ต่อแหล่ง |
| 🤝 | **Perturbation self-test** | ปิดเลนส์ / เขย่า / ทำเสียง แล้วดูว่าแหล่งนั้นตอบสนองจริงมั้ย |
| 🌐 | **100% offline** | ไม่ต่อเน็ต ไม่โหลด CDN / font / analytics ภายนอก |
| 📦 | **Single file** | ไฟล์เดียว ไม่ต้อง build ไม่มี dependency |
| 🌓 | **Dark HUD UI** | ธีม HUD มืด สองภาษา TH/EN |
| ♿ | **Accessible** | ใช้ keyboard ได้, reduced-motion, responsive |

### เริ่มใช้งาน

```bash
# 1) ดาวน์โหลดไฟล์เดียว (หรือ clone repo)
git clone https://github.com/<your-username>/d16-entropy-roller.git

# 2) (แนะนำ) ตรวจสอบลายเซ็น PGP — ดูหัวข้อด้านล่าง
gpg --verify d16-entropy-roller.html.asc d16-entropy-roller.html

# 3) เปิดใช้งาน — ไม่ต้องมี server ไม่ต้อง build
#    ดับเบิลคลิกที่ไฟล์ หรือ:
xdg-open d16-entropy-roller.html      # Linux
open d16-entropy-roller.html          # macOS
```

เพื่อความ isolated สูงสุด ก๊อปไฟล์ไปเครื่อง air-gapped (เช่น Tails OS) แล้วเปิดที่นั่น ไม่ต้องต่อเน็ตและไม่มีการต่อเน็ตในทุกกรณี

### กลไกการทำงาน

ทุกครั้งที่ทอย เครื่องมือประกอบ message จาก **accumulator ของแหล่งที่เลือก** XOR กับ CSPRNG ชุดใหม่ (เมื่อเลือก CSPRNG) บวก counter และ hi-res timestamp แล้ว hash ด้วย SHA-256 จากนั้นตัด **4 บิตล่าง (low nibble)** ของ byte แรก:

```text
material = selected_pool ⊕ csprng(32 bytes) ‖ counter(4) ‖ hi-res-time(8)
digest   = SHA-256(material)
value    = digest[0] & 0x0F        // 0–15 ไม่มี modulo bias (256 ÷ 16 ลงตัว)
pool    ⊕= digest                  // hash-chain: ทอยถัดไปไม่ซ้ำแม้ไม่มี sensor
```

**ทำไม `& 0x0F` ไม่มี bias:** byte สม่ำเสมอ 0–255 และ 256 หาร 16 ลงตัวพอดี ⇒ 4 บิตล่าง uniform เป๊ะบน 0–15 ไม่ต้องทำ rejection

**กฎที่ค้ำความปลอดภัย:** ถ้ามี **อย่างน้อย 1** แหล่งที่สุ่มจริงและอิสระ ผลรวมหลัง XOR/hash ก็ยัง uniform — แหล่งอ่อนทำร้ายไม่ได้

**มิกซ์เฉพาะที่เลือก:** มิกซ์เฉพาะแหล่งที่คุณติ๊กเลือกใน chips เท่านั้น ส่วน `input-timing` แสดงไว้เพื่อวินิจฉัยแต่ไม่เข้ามิกซ์

> animation ตอนหมุนใช้ `Math.random()` เพื่อความสวย **เท่านั้น** — ค่าที่ลงจริงมาจาก extractor ข้างบนเสมอ

### แหล่ง noise ของอุปกรณ์

| Source | ฟิสิกส์ | API | คุณภาพ | ข้อควรระวัง |
|---|---|---|---|---|
| **System CSPRNG** | OS entropy pool | `crypto.getRandomValues()` | baseline, active เสมอ | สม่ำเสมอ ~100% แต่ verify กระบวนการเองไม่ได้ |
| **Camera** | thermal / shot noise ของ image sensor | `getUserMedia` → canvas → **frame-diff LSB** | สูง | denoise/auto-exposure ลบ noise ได้ → ใช้ผลต่างเฟรม |
| **Microphone** | Johnson-Nyquist noise ของ ADC | `getUserMedia` (ปิด suppression) → `AnalyserNode` | ดี | ปิด `noiseSuppression / AGC / echoCancellation` |
| **Motion** | MEMS thermal + การเคลื่อนไหวมนุษย์ | `DeviceMotionEvent` | ดี | มือถือ; iOS ต้องกด enable |
| **Input timing** | jitter การแตะ/เมาส์ | pointer / key events | อ่อน | display only · ไม่เข้ามิกซ์ (browser clamp resolution กัน Spectre) |

camera / mic / motion คือแหล่งที่มี physical entropy จริง เป็นแบบ opt-in (browser จะถามสิทธิ์ตอน enable แต่ละตัว) ส่วน CSPRNG active เสมอเป็น baseline ทางสถิติ

### ตัวชี้วัด

แต่ละแหล่งที่ active แสดงค่าตรวจสอบ real-time:

- **min-entropy (bits/byte)** — ประเมินความสุ่มจริงต่อ byte (MCV) ยิ่งใกล้ 8 ยิ่งดี
- **RCT** *(Repetition Count Test)* — จับ source ที่ **ค้าง/ตาย** (ค่าซ้ำติดกันเกินเกณฑ์)
- **APT** *(Adaptive Proportion Test)* — จับ **ความเอียง/bias** ในหน้าต่าง 512 sample
- **ACTIVITY** — ความแปรปรวนระยะสั้น ใช้คู่กับ perturbation self-test

> **ขอบเขตที่ซื่อสัตย์:** ตัวชี้วัดพวกนี้ **falsify ได้** (พิสูจน์ว่าเสีย) แต่ **verify ไม่ได้** ว่า stream เป็น physical แท้หรือ PRNG ปลอมที่เนียน — เพราะ CSPRNG ดี ๆ ผ่านทุก test อยู่แล้ว การตรวจที่แข็งสุดคือ **perturbation self-test** ที่ *คุณ* รบกวน sensor เองแล้วดูว่า stream ตอบสนอง (oracle อยู่นอกเครื่อง)

### ตรวจสอบลายเซ็น — PGP

ทุก release เซ็นด้วย OpenPGP key ของผู้ดูแล ตรวจสอบก่อนเชื่อไฟล์ที่ดาวน์โหลดมาทุกครั้ง

```bash
# 1) นำเข้า public key (แนบมาในชื่อ chollatis-bitcoiner-pubkey.asc)
gpg --import chollatis-bitcoiner-pubkey.asc

# 2) ยืนยันว่า fingerprint ตรงกันเป๊ะ:
gpg --fingerprint chon_tit@hotmail.com
#    ค่าที่คาดหวัง:
#    EEFC F3F0 928D 0199 BA7E  56EC 2DB5 4085 AB23 3A47

# 3) ตรวจสอบลายเซ็น detached ของตัวแอป
gpg --verify d16-entropy-roller.html.asc d16-entropy-roller.html
#    มองหาข้อความ: "Good signature from Chollatis Maneewong - Bitcoiner"
```

| ข้อมูลกุญแจ | ค่า |
|---|---|
| Identity | `Chollatis Maneewong - Bitcoiner <chon_tit@hotmail.com>` |
| Algorithm | ed25519 (curve25519) |
| Fingerprint | `EEFC F3F0 928D 0199 BA7E 56EC 2DB5 4085 AB23 3A47` |

> "Good signature" ยืนยันแค่ว่าไฟล์ตรงกับที่ผู้ดูแลเซ็น **ไม่ได้** รับประกันความเหมาะสมกับการใช้งานใด ๆ — ขอบเขตความปลอดภัยใน [SECURITY.md](SECURITY.md) ยังมีผล

### ตรวจสอบว่าออฟไลน์จริง

อย่าเชื่อ ให้ตรวจเอง:

1. **อ่าน source** — ไฟล์เดียวอ่านออก ค้นหา `http`, `fetch`, `XMLHttpRequest`, `<script src` ได้เลย ไม่มีตัวชี้ออกนอกไฟล์
2. **ตัดเน็ต / ถอดสาย** แล้วใช้งาน — ทำงานครบทุกอย่าง
3. **เปิด DevTools → แท็บ Network** โหลดใหม่แล้วทอย — ไม่มี request ออกเลย

### การรองรับ & สิทธิ์

| ฟีเจอร์ | Chrome/Edge | Firefox | Safari (iOS) | Tor Browser / Tails |
|---|---|---|---|---|
| CSPRNG roll | ✅ | ✅ | ✅ | ✅ |
| Camera / Mic | ✅ prompt | ✅ prompt | ✅ prompt | ⚠️ มักถูกบล็อก |
| Motion / shake | – | – | ✅ (กด enable) | – |

> บน **Tails / Tor Browser** กล้อง/ไมค์มักถูกบล็อกเพื่อ privacy → แหล่งพวกนี้จะขึ้น `DENIED` **เป็นเรื่องปกติ ไม่ใช่ bug** — ตัว tool ยังทำงานครบด้วย CSPRNG baseline ทั้งนี้ `crypto.subtle` และ `getUserMedia` ต้องการ *secure context* ซึ่ง `file://` เข้าเงื่อนไขบนเบราว์เซอร์ยุคใหม่

### คำถามที่พบบ่อย (FAQ)

**ถาม: ใช้สร้าง seed จริงได้มั้ย?**
ไม่ได้ ดูคำเตือนด้านบนและ [SECURITY.md](SECURITY.md) สำหรับเงินจริงใช้ลูกเต๋าจริง + extractor ที่ตรวจสอบได้

**ถาม: มี CSPRNG ดีอยู่แล้ว ผสม sensor ทำไม?**
เพื่อสาธิต defense-in-depth (คุณสมบัติ "1 ใน N อิสระ") และ *แสดง* device noise อย่างโปร่งใส มันยกพื้น floor กัน bug ของ CSPRNG แบบไม่ตั้งใจได้ แต่ไม่ชนะ deep backdoor

**ถาม: ทำไมต้อง D16 (hex)?**
ลูกเต๋า 16 หน้า map เป็น hex nibble (4 บิต) พอดี ไม่มี waste ไม่มี modulo bias — สะอาดที่สุดในบรรดาลูกเต๋าทั่วไป

**ถาม: animation มีผลต่อผลลัพธ์มั้ย?**
ไม่มี — เป็นแค่ความสวย ค่าที่ลงมาจาก SHA-256 เสมอ

### ขอบเขตความปลอดภัย

เครื่องมือนี้ตั้งใจให้เป็น **educational simulator** เท่านั้น trust model เต็ม สิ่งที่ป้องกัน/ไม่ป้องกัน และวิธีรายงานปัญหา อยู่ใน **[SECURITY.md](SECURITY.md)** โปรดอ่านก่อนสรุปเรื่องคุณภาพ entropy

### สัญญาอนุญาต

เผยแพร่ภายใต้ **MIT License** — ดู [LICENSE](LICENSE)

<p align="right"><sub><a href="#thai">⤴ บนสุด</a> · <a href="#english">🇬🇧 Read in English</a></sub></p>

---

<p align="center">
  <sub>Built by <strong>Chollatis Maneewong</strong> · Chollatis Bitcoiner · <a href="https://learning.chontit.win">learning.chontit.win</a></sub><br>
  <sub><em>"Don't Trust, Verify."</em></sub>
</p>
