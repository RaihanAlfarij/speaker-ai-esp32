# Catatan serah-terima — sesi Firmware & Hardware

Tempel/rujuk file ini di awal sesi firmware & hardware.

## Siapa & konteks
- Raihan, Hardware Engineer (terbiasa wiring, multimeter, Arduino IDE, serial monitor, level shifting, RS232/RS485). Istilah web/devops (Docker, reverse proxy, DNS) perlu dijelaskan singkat.
- Dokumen utama: `docs/PRD.md`. Pinout: `docs/hardware/pinout.md`. Daya: `docs/hardware/power.md`. BOM: `docs/hardware/bom.md`.

## Keputusan yang sudah diambil
- Firmware base xiaozhi-esp32, board profile `bread-compact-wifi`, OLED SH1106 128×64.
- Firmware: fork `RaihanAlfarij/xiaozhi-esp32` sudah dipasang sebagai submodule `firmware/xiaozhi-esp32` (branch kerja `speaker-ai`, remote `upstream` → `78/xiaozhi-esp32`).
- Konten: setiap sesi kerja direkam minim usaha untuk daily vlog (HP 1 tampak atas tetap, HP 2 close-up untuk momen kunci). Tepuk tangan di momen penting.
- Server: xiaozhi-esp32-server self-hosted (fase 2), Whisper (Groq) + Gemini Flash + Edge TTS id-ID.
- Daya: 1S2P + BMS 1S + IP2312 (RICHG 68k) + load sharing + MT3608 (resistor tetap 75k/10k).
- BT ke speaker lain ditunda.

## Posisi sekarang
Fase 1 belum dimulai. Semua komponen inti sudah ada.

## Langkah berikutnya (fase 1)
1. Wiring breadboard sesuai pinout (ring WS2812 boleh belum dipasang). Touch: jumper pad A → aktif LOW.
2. Unduh firmware `bread-compact-wifi` dari GitHub Releases xiaozhi-esp32 → flash (Flash Download Tool di Windows / esptool).
3. Sambung ke hotspot `Xiaozhi-XXXX`, isi WiFi, daftar perangkat di console xiaozhi.me (+ Add Device, masukkan kode).
4. Tes ngobrol. Catat hasil di `docs/devlog.md`.
5. Kalau OLED bergeser/garis di pinggir → firmware siap pakai mungkin untuk SSD1306; perbaiki saat build sendiri (opsi SH1106).
6. Kalau ada modul tidak jalan → buat sketch tes Arduino per modul (I2C scan/OLED, sinus 1 kHz MAX98357, level meter INMP441, animasi WS2812).

## Lingkungan
- Laptop Windows (Git Bash / MINGW64). Repo di `D:\Data raihan\Speaker AI Esp32`, GitHub `RaihanAlfarij/speaker-ai-esp32`.

## Belum diputuskan
- Server di PC rumah / STB / VPS
- 1S2P vs 1S4P, driver final
