# 0001 — Firmware base: xiaozhi-esp32

- **Tanggal:** 2026-11-17
- **Status:** Diterima

## Konteks
Butuh speaker asisten dengan AI generatif (tanya-jawab bebas seperti chatbot), bisa dikustom, ditambah plugin (Spotify, YouTube), dan terhubung ke smart home.

## Pilihan
1. **xiaozhi-esp32** — firmware ESP-IDF (C++), fokus LLM, MCP untuk kontrol perangkat, dukung OLED & board breadboard (`bread-compact-wifi`), lisensi MIT, server open-source tersedia.
2. **ESPHome + Home Assistant (Voice PE)** — sangat baik untuk kontrol rumah; contoh build yang dicoba (smarthomecircle) hanya memakai Whisper + Piper tanpa LLM. LLM bisa ditambahkan di HA, tapi perangkat jadi bergantung pada HA.

## Keputusan
Pakai **xiaozhi-esp32** sebagai base, di-fork, lalu dimodifikasi. Home Assistant tetap bisa dihubungkan lewat MCP.

## Konsekuensi
- Hardware mengikuti pinout `bread-compact-wifi` agar firmware siap pakai langsung jalan.
- Butuh ESP-IDF ≥ 5.4 untuk build sendiri.
- Atribusi lisensi MIT xiaozhi wajib dijaga (juga disebut di konten).
