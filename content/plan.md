# Rencana Konten — "Bikin Speaker AI dari Nol"

Gaya: **daily tech vlog / build in public** — posting apa adanya setiap ada progres, konsisten, minim usaha. Kegagalan ikut jadi konten.

## 1. Tiga jalur

| Jalur | Kapan | Platform & format | Template |
|---|---|---|---|
| **A. Daily build vlog** ("Hari ke-N") | Setiap sesi kerja | IG Reels → YouTube Shorts → TikTok · 9:16, 1080×1920, 30–60 dtk | [`vlog/_template-hari.md`](vlog/_template-hari.md) |
| **B. Carousel milestone** | Saat milestone tercapai | IG feed → Threads · 4:5, 1080×1350 | [`episodes/_template.md`](episodes/_template.md) |
| **C. Tutorial YouTube** | Setelah v1 jadi (fase 7) | YouTube · 16:9, panjang | dibuat nanti dari catatan devlog |

- "Hari ke-N" = urutan **sesi kerja**, bukan tanggal. Tidak wajib upload tiap hari.
- Tutorial (C) direkam dari **rakitan kedua** yang mulus, bukan dari rakitan pertama.

## 2. Struktur 1 vlog (jalur A)

| Bagian | Durasi | Angle | Isi |
|---|---|---|---|
| Hook | 0–3 dtk | HP 2 (close-up/wajah) | Hasil atau masalah duluan: "Akhirnya bunyi!" / "Kenapa nggak nyala?" |
| Judul | 3–5 dtk | Teks | "Hari ke-N bikin speaker AI" |
| Proses | 5–40 dtk | HP 1 (atas, timelapse) + rekam layar | 3–5 potongan cepat |
| Momen kunci | 40–50 dtk | HP 2 (close-up) | Berhasil / gagal pertama kali |
| Penutup | 50–60 dtk | Wajah / teks | Pelajaran singkat + "besok: …" |

Suara: voice-over direkam setelahnya di ruang sepi, atau cukup teks + musik.

### Konten cadangan (hari tanpa progres)
- **Masalah hari ini** — jelaskan error, minta saran penonton
- **Kenalan komponen** — 1 modul, 15 detik
- **Ngobrol random dengan AI-nya** — setelah speaker bisa bicara

## 3. Rencana vlog

| Hari | Isi | Fase | Status |
|---|---|---|---|
| 1 | Persiapan: komponen & alat disusun rapi (knolling, tampak atas) | 0 | ⬜ |
| 2 | Setup proyek dengan bantuan Claude: PRD, folder, GitHub (rekam layar) | 0 | ⬜ |
| 3+ | Ikuti progres fase 1 dst. (wiring, flash, ngobrol pertama, …) | 1+ | ⬜ |

## 4. Milestone carousel (jalur B)

| Ep | Topik | Fase | Hook Reels pendamping | Isi carousel | Status |
|---|---|---|---|---|---|
| 1 | Kenapa bikin ini | 0 | "Google Nest Mini, tapi rakit sendiri?" | Target fitur + arsitektur | ⬜ |
| 2 | Belanja komponen | 0 | Unboxing semua komponen | Daftar komponen + harga | ⬜ |
| 3 | Bongkar speaker BT rusak | 5 | "Speaker rusak ini jangan dibuang" | Cara uji driver & passive radiator | ⬜ |
| 4 | Desain power | 5 | Spot weld baterai | IP2312, BMS, MT3608 + tips aman | ⬜ |
| 5 | Mic + amp pertama | 1 | Bunyi pertama dari breadboard | Wiring I2S | ⬜ |
| 6 | Flash firmware | 1 | Ngobrol pertama dengan AI | Alur suara → server → jawaban | ⬜ |
| 7 | Bisa Bahasa Indonesia | 2 | "Akhirnya ngerti bahasa kita" | Whisper, LLM, TTS + biaya | ⬜ |
| 8 | OLED + touch + saklar mic | 3 | Wajah & ekspresi di OLED | UI + kenapa saklar mic fisik | ⬜ |
| 9 | Desain box + 3D print | 5 | Timelapse cetak casing | Hitungan box & passive radiator | ⬜ |
| 10 | Kontrol IoT | 4 | "Nyalakan lampu" pakai suara | MCP/MQTT ke modul | ⬜ |
| 11 | Putar musik | 4 | Minta lagu ke speaker | Cara kerja streaming | ⬜ |
| 12 | Reveal final | 6 | Adu suara vs Nest Mini | Rekap, total biaya, pelajaran | ⬜ |

Urutan tayang tidak harus sama dengan urutan pengerjaan.

## 5. Setup kamera (2 HP + 1 tripod + 1 holder)

**HP 1 — tripod, tampak atas (angle utama)**
- Tripod di belakang/samping meja, kepala ditekuk ke bawah (hampir tegak lurus; 60–75° juga oke).
- Rekam **horizontal 4K** → bisa di-crop 9:16 untuk Reels, versi lebar untuk YouTube.
- Posisi **tetap**; cukup tekan rekam tiap mulai sesi.

**HP 2 — holder meja, close-up (pendukung)**
- Rendah di samping, 30–45°, ke tangan/breadboard. **Vertikal 9:16**.
- Dipindah hanya untuk momen kunci (OLED menyala, LED ring, bunyi pertama, multimeter) atau untuk wajah/reaksi.

**Laptop — rekam layar**
- Win + Alt + R (Xbox Game Bar) atau OBS. Zoom browser/editor 125–150 %.

**Trik gambar**
- Cahaya jendela dari samping, bukan dari belakang; malam: lampu meja dipantulkan ke dinding.
- Kunci fokus & exposure (tekan lama di layar, AE/AF lock).
- Anti-flicker 50 Hz atau shutter 1/50–1/100.
- Mode pesawat + Jangan Ganggu, HP sambil dicas, cek sisa memori.
- Alas meja polos (cutting mat / karton gelap).
- **Tepuk tangan** di awal (sinkron 2 kamera) dan di momen penting (penanda saat edit).

## 6. Checklist stok footage (B-roll)
- [ ] Close-up tiap komponen
- [ ] Tangan memasang jumper / solder
- [ ] OLED menyala pertama kali
- [ ] Animasi ring LED
- [ ] Serial monitor bergulir
- [ ] Timelapse solder / wiring
- [ ] Angka di multimeter
- [ ] Speaker bergetar / bunyi
- [ ] Rekam layar: flash firmware, GitHub, ngobrol dengan Claude

## 7. File & penyimpanan
- Nama file: `YYYYMMDD_angle_keterangan.mp4` (contoh `20261117_atas_knolling.mp4`). Angle: `atas`, `close`, `layar`, `wajah`.
- Footage mentah **di luar git** (folder/drive terpisah). `content/**/raw/` sudah di-ignore sebagai cadangan.
- Hasil final kecil (thumbnail, slide PNG) boleh masuk git.

## 8. Checklist sebelum posting
- [ ] Tidak ada password WiFi, API key, `.config.yaml`, token, email pribadi di layar
- [ ] Atribusi: "firmware berbasis xiaozhi-esp32 (open-source, MIT)" bila relevan
- [ ] Caption + auto-caption dicek ejaannya
- [ ] Musik dari pustaka platform (aman hak cipta)
- [ ] Catat di `docs/devlog.md`: hari ke-N, link post

## 9. Edit
CapCut: potong cepat, auto-caption, teks singkat, musik bawaan. Target per sesi kerja: **1 vlog + 3–5 foto** untuk carousel.

## Belum diputuskan
- Nama/persona asisten
- Tampil muka atau tidak; voice-over atau teks saja
