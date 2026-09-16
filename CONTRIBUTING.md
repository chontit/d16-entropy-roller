# Contributing / การมีส่วนร่วม

Thanks for your interest. This project has an unusually strict design contract because it lives in a security-adjacent space.
ขอบคุณที่สนใจ โปรเจกต์นี้มี design contract ที่เข้มเป็นพิเศษ เพราะอยู่ในพื้นที่ security-adjacent

## Non-negotiable constraints / ข้อกำหนดที่ต่อรองไม่ได้

1. **Single file, offline, zero dependencies.** No external scripts, fonts, CDNs, analytics, or network calls of any kind. A PR that adds one will be closed.
   **ไฟล์เดียว ออฟไลน์ ไม่มี dependency** ห้าม script/font/CDN/analytics/network ภายนอกทุกชนิด
2. **No seed / key / wallet functionality — ever.** This tool rolls dice. It must not gain any path that turns rolls into a mnemonic, key, or address.
   **ห้ามมี seed/key/wallet เด็ดขาด** — เครื่องนี้ทอยลูกเต๋าเท่านั้น ห้ามเพิ่มเส้นทางแปลงผลเป็น mnemonic/key/address
3. **Honesty over polish.** Any change touching randomness or indicators must keep the security scope accurate. Do not overstate what the tool guarantees.
   **ซื่อสัตย์มาก่อนความสวย** การแก้ที่แตะ randomness/indicator ต้องคงความถูกต้องของ security scope ห้าม overstate

## Good contributions / สิ่งที่ยินดีรับ

- Bug fixes in extraction, health tests, or source handling (with a clear explanation of correctness).
  แก้บั๊กใน extraction / health test / source handling พร้อมอธิบายความถูกต้อง
- Accessibility, responsiveness, and reduced-motion improvements.
  ปรับ accessibility / responsive / reduced-motion
- Additional *transparent* device-noise sources, each with its own indicators and honest caveats.
  เพิ่มแหล่ง device-noise แบบโปร่งใส พร้อม indicator และ caveat ที่ซื่อสัตย์
- Translations and documentation clarity.
  แปลภาษาและปรับเอกสารให้ชัดขึ้น

## Process / ขั้นตอน

1. Open an issue describing the change before large PRs.
   เปิด issue อธิบายก่อนสำหรับ PR ใหญ่
2. Keep diffs readable; the tool is meant to be auditable by reading the source.
   ทำ diff ให้อ่านง่าย ตัว tool ต้อง audit ได้ด้วยการอ่าน source
3. Test on at least Chromium + Firefox, and note mobile behavior if relevant.
   ทดสอบอย่างน้อย Chromium + Firefox และระบุพฤติกรรมบนมือถือถ้าเกี่ยวข้อง
4. For anything security-relevant, follow [SECURITY.md](SECURITY.md) and report privately first.
   เรื่อง security ให้ทำตาม [SECURITY.md](SECURITY.md) และรายงานส่วนตัวก่อน

## License / สัญญาอนุญาต

By contributing, you agree your work is licensed under the project's [MIT License](LICENSE).
การมีส่วนร่วมถือว่ายอมรับให้ผลงานอยู่ภายใต้ [MIT License](LICENSE) ของโปรเจกต์
