# Server AI

Berbasis [xiaozhi-esp32-server](https://github.com/xinnan-tech/xiaozhi-esp32-server) (Python, Docker), dijalankan di jaringan sendiri (fase 2).

## Rencana konfigurasi
| Bagian | Pilihan |
|---|---|
| STT | Whisper large-v3-turbo via Groq (mendukung Bahasa Indonesia) |
| LLM | Gemini Flash / LLM lain berformat API OpenAI |
| TTS | Edge TTS `id-ID-ArdiNeural` / `id-ID-GadisNeural` |
| Plugin | `plugins/` di folder ini → disalin/di-mount ke `plugins_func/` server |

## Isi folder
- `config/` — contoh konfigurasi **tanpa** API key (mis. `config.example.yaml`). File asli berisi kunci (`.config.yaml`, `.env`) sudah di-ignore.
- `plugins/` — plugin buatan sendiri: musik (YouTube/Spotify), IoT (MQTT), Home Assistant.

> ⚠️ Jangan pernah commit API key.
