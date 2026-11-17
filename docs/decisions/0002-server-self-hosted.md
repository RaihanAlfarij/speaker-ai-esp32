# 0002 — Server AI self-hosted

- **Tanggal:** 2026-11-17
- **Status:** Diterima

## Konteks
Server resmi xiaozhi.me gratis untuk pribadi tapi bukan untuk komersial, dan ASR default-nya (SenseVoice) tidak mendukung Bahasa Indonesia. Plugin buatan sendiri juga butuh server yang bisa diubah.

## Keputusan
- Fase 1: pakai xiaozhi.me hanya untuk membuktikan hardware & firmware jalan.
- Fase 2 dst.: **xiaozhi-esp32-server** (Python, Docker) di jaringan sendiri.
  - STT: Whisper large-v3-turbo (Groq) — multibahasa termasuk ID
  - LLM: Gemini Flash / LLM lain berformat API OpenAI
  - TTS: Edge TTS id-ID (Ardi/Gadis)
  - Plugin: `plugins_func/` → kode kita di `server/plugins/`

## Konsekuensi
- Biaya bulanan rendah (target < Rp50 rb), tapi butuh mesin yang menyala (PC/STB/VPS).
- API key disimpan di `.config.yaml`/`.env` yang **tidak** di-commit (lihat `.gitignore`).
