# Firmware

Firmware berbasis [xiaozhi-esp32](https://github.com/78/xiaozhi-esp32) (MIT) yang dimodifikasi.

## Setup (sekali saja)
1. Fork `78/xiaozhi-esp32` ke akun GitHub kamu.
2. Dari root repo ini (folder ini masih berisi README saja):
   ```bash
   git submodule add https://github.com/<akun-kamu>/xiaozhi-esp32.git firmware/xiaozhi-esp32
   cd firmware/xiaozhi-esp32
   git remote add upstream https://github.com/78/xiaozhi-esp32.git
   ```
3. Buat branch kerja, mis. `speaker-ai`, untuk semua modifikasi.

## Build (ESP-IDF ≥ 5.4)
```bash
idf.py set-target esp32s3
idf.py menuconfig   # Board: bread-compact-wifi, OLED: SH1106 128x64, bahasa, server URL
idf.py build flash monitor
```

## Rencana modifikasi
- Ring WS2812 (GPIO 21) sebagai indikator mendengar/berpikir/bicara
- Touch 2: tap = Vol+, tahan = Vol−
- UI OLED versi sendiri
- Server URL → server sendiri (fase 2)

Pinout: [`../docs/hardware/pinout.md`](../docs/hardware/pinout.md)
