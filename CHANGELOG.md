# Changelog

All notable changes to this project are documented here.
The format follows [Keep a Changelog](https://keepachangelog.com/) and
[Semantic Versioning](https://semver.org/).

รูปแบบอ้างอิง Keep a Changelog และ Semantic Versioning

---

## [1.0.1] — 2026-09-17

### Changed / เปลี่ยน
- **Shake-to-roll threshold raised** (14 → 25) so only a firm, deliberate shake triggers a roll — prevents accidental rolls from small movements.
  **เพิ่มเกณฑ์ shake-to-roll** (14 → 25) ต้องเขย่าแรงและตั้งใจจริงถึงจะทอย กันการทอยพลาดจากการขยับเล็กน้อย

### Added / เพิ่ม
- **Shake-to-roll on/off toggle** on the main screen (shown on motion-capable devices), independent of the Motion entropy source — turn shake off while still using motion noise in the mix. Turning it on ensures the motion listener is running.
  **ปุ่ม toggle เปิด/ปิด shake-to-roll** บนหน้าหลัก (แสดงบนอุปกรณ์ที่มี motion) แยกอิสระจาก Motion entropy source — ปิดการเขย่าได้โดยยังใช้ motion noise ในมิกซ์ต่อ
- **Pipeline flowchart** added to the in-tool *How it works* section — shows sources → XOR pool → SHA-256 → D16 roll.
  **เพิ่ม flowchart แผนผังระบบ** ในหัวข้อ *How it works* ของตัว tool — แสดง sources → XOR pool → SHA-256 → ผล D16

---

## [1.0.0] — 2026-09-16

Initial public release. / รีลีสสาธารณะครั้งแรก

### Added / เพิ่ม
- Single-file, fully-offline D16 (16-sided) dice simulator with faceted-gem HUD tumble animation.
  เครื่องจำลองการทอย D16 ไฟล์เดียว ออฟไลน์เต็ม พร้อม animation แบบ faceted-gem HUD
- **Quick-select entropy chips** (`CSPRNG` / `Cam` / `Mic` / `Motion`) above the roll button:
  tap to include a source in the mix without opening the diagnostics drawer. Enabling a sensor
  chip requests its permission; disabling it stops capture (camera/mic tracks are actually
  released) and removes it from the mix. A guard keeps at least one source selected.
  **ชิปเลือกแหล่ง entropy แบบเร็ว** เหนือปุ่มทอย ติ๊กเพื่อเพิ่มเข้ามิกซ์โดยไม่ต้องเปิดแผงเบื้องหลัง
  ปิดติ๊ก = หยุดใช้และปล่อยกล้อง/ไมค์จริง และมี guard บังคับให้เหลืออย่างน้อย 1 แหล่ง
- SHA-256 extractor with unbiased `& 0x0F` nibble derivation and hash-chained pool; mixes
  **only the sources the user has selected**, and the roll hint shows the live `A ⊕ B ⊕ …` set.
  extractor SHA-256 ตัด nibble แบบไม่มี bias, hash-chain pool, มิกซ์เฉพาะแหล่งที่เลือก และ hint แสดงชุดที่ใช้จริง
- Multi-source entropy harvesting: system CSPRNG, camera (frame-diff LSB), microphone
  (ADC LSB, suppression disabled), motion/accelerometer, and input timing (display only).
  ดึง entropy หลายแหล่ง: CSPRNG, กล้อง, ไมค์, motion, และ input timing (display only)
- Shake-to-roll on mobile via `DeviceMotionEvent`, with a capability probe that reverts motion
  to `N/A` when no sensor is present (e.g. desktops).
  เขย่าเพื่อทอยบนมือถือ พร้อมตรวจว่ามี motion sensor จริง ถ้าไม่มีจะกลับเป็น `N/A`
- Live source panel with per-source state (`ACTIVE / DENIED / OFF / N/A`), an `IN MIX` tag,
  enable/disable toggles kept in sync with the chips, a min-entropy meter (Most-Common-Value),
  and RCT / APT health lamps.
  แผงแหล่งข้อมูล real-time พร้อมป้าย `IN MIX`, ปุ่มเปิด/ปิดที่ sync กับชิป, min-entropy และไฟ RCT/APT
- Perturbation self-test for camera, microphone, and motion.
  perturbation self-test สำหรับกล้อง/ไมค์/motion
- Collapsible bilingual (TH/EN) "how it works" documentation inside the tool.
  คำอธิบายกลไกสองภาษาแบบพับเก็บในตัว tool
- Accessibility: keyboard operation, `prefers-reduced-motion` support, responsive layout.
  รองรับ keyboard, reduced-motion, responsive

### Security / ความปลอดภัย
- Explicit in-tool and in-repo notices that this is an educational simulator and must not
  be used to generate real seeds or keys. See `SECURITY.md`.
  แจ้งชัดทั้งในตัว tool และ repo ว่าเป็น educational simulator ห้ามใช้สร้าง seed/กุญแจจริง
- No network activity of any kind; no external scripts, fonts, or analytics.
  ไม่มี network activity ใด ๆ ไม่มี script/font/analytics ภายนอก

[1.0.1]: https://github.com/<your-username>/d16-entropy-roller/releases/tag/v1.0.1
[1.0.0]: https://github.com/<your-username>/d16-entropy-roller/releases/tag/v1.0.0
