# Speaker AI ESP32

Speaker pintar portable berbasis **ESP32-S3-N16R8** yang bisa diajak ngobrol (AI, Bahasa Indonesia & Inggris), memutar musik, dan mengontrol perangkat smart home. Hardware dirakit dari nol; firmware berbasis [xiaozhi-esp32](https://github.com/78/xiaozhi-esp32) (MIT) yang dimodifikasi; otak AI di server sendiri ([xiaozhi-esp32-server](https://github.com/xinnan-tech/xiaozhi-esp32-server)).

📄 Dokumen utama: [`docs/PRD.md`](docs/PRD.md)

## Status

| Fase | Status |
|---|---|
| 0 — Persiapan (komponen, repo, PRD) | 🟡 berjalan |
| 1 — Breadboard + firmware siap pakai | ⚪ belum |
| 2 — Server AI sendiri + Bahasa Indonesia | ⚪ belum |
| 3 — Modifikasi firmware (LED, touch, OLED) | ⚪ belum |
| 4 — Plugin musik & IoT | ⚪ belum |
| 5 — Daya portable + casing | ⚪ belum |
| 6 — Rilis v1.0 | ⚪ belum |

Log progres: [`docs/devlog.md`](docs/devlog.md)

## Struktur repo

```
docs/        PRD, hardware (pinout, BOM, power), keputusan, devlog, catatan sesi
hardware/    skema & desain casing
firmware/    fork xiaozhi-esp32 (git submodule)
server/      konfigurasi xiaozhi-esp32-server + plugin buatan sendiri
content/     rencana konten & bahan per episode
```

## Lisensi

MIT — lihat [`LICENSE`](LICENSE). Firmware turunan xiaozhi-esp32 tetap mengikuti lisensi MIT proyek aslinya.
