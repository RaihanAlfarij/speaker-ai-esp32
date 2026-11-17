# 0004 — Bluetooth ke speaker lain ditunda

- **Tanggal:** 2026-11-17
- **Status:** Diterima

## Konteks
F7 (kirim audio ke speaker Bluetooth lain) butuh A2DP = Bluetooth Classic. ESP32-S3 hanya mendukung Bluetooth LE.

## Keputusan
Tunda F7. Fokus fitur lain dulu.

## Opsi nanti
1. ESP32 klasik (WROOM-32) sebagai co-processor A2DP source, audio dikirim dari S3 lewat I2S.
2. Migrasi ke ESP32-S31 (WiFi 6, BT 5.4 LE + Classic) di versi berikutnya.
3. Modul BT transmitter analog (paling mudah, kualitas/kontrol terbatas).
