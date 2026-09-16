# Assets to add before publishing / ไฟล์ที่ต้องเติมก่อนเผยแพร่

These are referenced by the docs but must be produced by you (they involve your private key or screenshots).
ไฟล์เหล่านี้ถูกอ้างในเอกสาร แต่ต้องสร้างเอง (เกี่ยวกับ private key หรือ screenshot)

## 1. Public key / กุญแจสาธารณะ
Export your signing public key to the repo root:
```bash
gpg --armor --export chon_tit@hotmail.com > chollatis-bitcoiner-pubkey.asc
```
Expected fingerprint / fingerprint ที่ถูกต้อง:
`EEFC F3F0 928D 0199 BA7E 56EC 2DB5 4085 AB23 3A47`

## 2. Detached signatures / ลายเซ็นแบบ detached
Sign the tool and the checksum file (do this in your offline signing environment):
```bash
gpg --armor --detach-sign d16-entropy-roller.html      # -> d16-entropy-roller.html.asc
gpg --armor --detach-sign SHA256SUMS                    # -> SHA256SUMS.asc
```

## 3. Checksums / ค่าตรวจสอบ
`SHA256SUMS` is already generated for the current build:
```
abd79798cd511db6f82edcd69f7385362d4b447fab721c816df4361bd3086596  d16-entropy-roller.html
```
Regenerate if you modify the HTML / สร้างใหม่ถ้าแก้ไฟล์ HTML:
```bash
sha256sum d16-entropy-roller.html > SHA256SUMS
```

## 4. Screenshots (optional) / ภาพหน้าจอ (ถ้ามี)
Add to `docs/` and reference them in the README hero, e.g.:
- `docs/screenshot-desktop.png`
- `docs/screenshot-mobile.png`

## 5. Replace placeholders / แทนที่ตัวยึด
Search the repo for `<your-username>` and replace with your GitHub handle:
```bash
grep -rn "<your-username>" .
```
