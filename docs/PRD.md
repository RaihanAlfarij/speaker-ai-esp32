# PRD — Speaker AI ESP32

| | |
|---|---|
| **Pemilik** | Raihan (Hardware Engineer) |
| **Status** | Draft v0.2 — fase 0 (komponen sudah lengkap, belum dirakit) |
| **Tanggal** | 17 November 2026 |
| **Repo** | `speaker-ai-esp32` (lisensi MIT) |

---

## 1. Ringkasan

Speaker pintar **kompak dan portable** berbasis **ESP32-S3-N16R8** yang bisa diajak ngobrol seperti chatbot AI (Bahasa Indonesia & Inggris, mendukung bahasa lain), memutar musik, dan mengontrol perangkat smart home. Hardware dirakit sendiri dari nol; firmware memakai base open-source **xiaozhi-esp32** yang kemudian dimodifikasi; "otak" AI dijalankan di **server sendiri** (xiaozhi-esp32-server) supaya bisa ditambah plugin (Spotify, YouTube, IoT).

Proses pembuatannya sekaligus dijadikan **seri konten** (IG feed carousel + Reels → Shorts, TikTok, Threads).

Acuan pengalaman: **Google Nest Mini gen 2** (suara jernih, tombol volume, saklar mic fisik, LED status, jawab pertanyaan) — tapi dengan AI generatif dan bisa dikustom penuh.

## 2. Tujuan

1. **Pribadi (sekarang):** speaker AI yang dipakai sendiri setiap hari.
2. **Konten:** dokumentasi build sebagai seri edukasi di media sosial.
3. **Nanti (opsional):** open-source di GitHub → jual modul kit → jual unit siap pakai.

Prinsip: **biaya bulanan seminimal mungkin**, setiap fase menghasilkan sesuatu yang bisa didemokan.

## 3. Ruang lingkup

### Masuk MVP
- Ngobrol dengan AI (tanya-jawab, "berpikir" sebelum menjawab) dalam Bahasa Indonesia & Inggris
- Wake word + tombol/sentuh untuk bicara
- WiFi dengan banyak SSID tersimpan
- OLED interaktif + ring LED status
- Putar musik (streaming dari server)
- Kontrol perangkat IoT buatan sendiri + Home Assistant (opsional)
- Baterai 1S + charging USB-C, casing kompak

### Ditunda (bukan MVP)
- **Koneksi ke speaker Bluetooth lain (F7)** — ESP32-S3 hanya punya BLE, tidak mendukung Bluetooth Classic/A2DP. Opsi nanti: ESP32 klasik sebagai co-processor A2DP, atau migrasi ke ESP32-S31 (punya BT Classic + LE Audio).
- EQ hardware (DSP TAS5805M), mic array dengan AEC hardware, PCB custom, produk komersial.

## 4. Kebutuhan fungsional

| ID | Fitur | Prioritas | Kriteria selesai |
|---|---|---|---|
| F1 | Interaksi AI | P1 | Bisa tanya-jawab bebas dalam B. Indonesia & Inggris, jawaban diucapkan; mode "berpikir" (model reasoning) bisa diaktifkan |
| F2 | Musik | P2 | Perintah suara "putar lagu X" → musik diputar dari server; pause/next/volume via suara & tombol |
| F2a | Akun Google / YouTube / Spotify | P2–P3 | YouTube (via server, jalur tidak resmi) dan Spotify (Spotify Connect, perlu Premium) bisa dipanggil sebagai plugin. "Google" = Gemini API + API Google lain via OAuth di server (Google Assistant tidak bisa ditanam) |
| F3 | Kontrol IoT | P2 | Perintah suara mengontrol modul IoT sendiri (MQTT) dan/atau entitas Home Assistant (MCP) |
| F4 | WiFi multi-SSID | P1 | Simpan ≥3 SSID, auto-pindah ke jaringan yang tersedia, provisioning lewat HP |
| F5 | Kompak & portable | P2 | Berbasis baterai 1S2P, casing 3D print, ukuran setara speaker portable kecil |
| F6 | OLED interaktif | P1 | Tampil status (WiFi, baterai, mendengar/bicara), teks/emoji, menu sederhana |
| F7 | Ke speaker Bluetooth lain | **Ditunda** | Lihat bagian 3 |
| F8 | Suara jernih + atur bass/treble | P2 | Tidak pecah di volume maksimal; EQ software (bass/treble) bisa diatur via suara/menu |

## 5. Kebutuhan non-fungsional

| Aspek | Target |
|---|---|
| Bahasa | Utama ID & EN; bahasa lain mengikuti kemampuan STT/LLM/TTS |
| Respons | Suara jawaban mulai terdengar ≲ 3 detik setelah selesai bicara (target awal) |
| Baterai | ≥ 8 jam pemakaian campuran (1S2P) |
| Privasi | Saklar mic **fisik** (memutus mic, bukan mute software); server milik sendiri |
| Biaya bulanan | < Rp50.000 untuk pemakaian pribadi (server di rumah + API murah) |
| Open-source | Kode & dokumentasi rapi, atribusi xiaozhi (MIT) dijaga |
| Keamanan baterai | Proteksi BMS, cutoff software ~3,3 V, NTC (versi final) |

## 6. Arsitektur

```
[Speaker ESP32-S3]  ⇄ WiFi ⇄  [Server sendiri]  →  [Layanan luar]
 mic, wake word,               STT (Whisper)        LLM API (Gemini, dll)
 amp+speaker, OLED,            LLM orchestrator     Musik (YouTube/Spotify)
 LED ring, touch               TTS (Edge id-ID)     IoT (MQTT / Home Assistant)
                               plugin & MCP
```

### 6.1 Lapis perangkat — firmware
- Base: **xiaozhi-esp32** (MIT), board profile `bread-compact-wifi`, OLED **SH1106 128×64**.
- Repo firmware = fork **`RaihanAlfarij/xiaozhi-esp32`**, dipasang sebagai git submodule `firmware/xiaozhi-esp32` (branch kerja `speaker-ai`, remote `upstream` → repo asli).
- Modifikasi rencana: ring WS2812 (status), 2 sensor touch (talk & volume tap/hold), UI OLED versi sendiri, bahasa default, server URL ke server sendiri.
- Toolchain: ESP-IDF ≥ 5.4 (VS Code + ekstensi ESP-IDF).

### 6.2 Lapis otak — server
- **xiaozhi-esp32-server** (Python, Docker) di PC/laptop/STB rumah; VPS opsional.
- STT: **Whisper large-v3-turbo via Groq** (multibahasa termasuk ID, sangat murah). Catatan: ASR default xiaozhi (SenseVoice) tidak mendukung Bahasa Indonesia.
- LLM: Gemini Flash (atau LLM lain yang kompatibel API OpenAI).
- TTS: Edge TTS `id-ID-ArdiNeural` / `id-ID-GadisNeural` (gratis, endpoint tidak resmi — ganti TTS berbayar bila komersial).
- Plugin: folder `plugins_func/` di server → kode plugin kita disimpan di `server/plugins/` repo ini.

### 6.3 Lapis integrasi
- **Musik:** YouTube → server mengambil stream audio → ESP32 memutar. Spotify → Spotify Connect (cspot) / Music Assistant, butuh Premium.
- **Smart home:** perangkat buatan sendiri via **MQTT**; Home Assistant via **MCP** (mis. integrasi HACS `ha-mcp-for-xiaozhi`).

## 7. Hardware

### 7.1 Komponen (status 17 Nov 2026)
Lihat detail di [`docs/hardware/bom.md`](hardware/bom.md).

| Komponen | Status |
|---|---|
| ESP32-S3-N16R8 devkit | ✅ ada |
| INMP441 (mic I2S) | ✅ ada |
| MAX98357A (amp I2S 3,2 W/4 Ω) | ✅ ada |
| Speaker 4 Ω 5–10 W neodymium | ✅ ada |
| OLED 1,3" SH1106 I2C | ✅ ada |
| Ring WS2812B 16 LED (5050) | ✅ ada |
| Sensor touch ×2 (TTP223) | ✅ ada |
| Modul charger IP2312, boost MT3608, BMS 1S | ✅ ada |
| Baterai 18650, komponen pasif (load sharing, resistor, kapasitor) | ⬜ cek `bom.md` |

### 7.2 Pinout
Mengikuti board `bread-compact-wifi` xiaozhi supaya firmware siap pakai langsung jalan. Detail: [`docs/hardware/pinout.md`](hardware/pinout.md).

| Fungsi | GPIO |
|---|---|
| Mic INMP441 WS / SCK / SD | 4 / 5 / 6 |
| Amp MAX98357 DIN / BCLK / LRC | 7 / 15 / 16 |
| OLED SDA / SCL | 41 / 42 |
| Touch 1 (talk) / Touch 2 (vol) | 47 / 40 |
| WS2812 data | 21 (modifikasi) |
| Lampu demo IoT | 18 |

### 7.3 Daya
1S2P 18650 → BMS 1S → IP2312 (charge ~2 A) ; rail sistem lewat load-sharing (Schottky + P-MOSFET) → MT3608 boost 5,0–5,1 V (resistor tetap) → amp + devkit. Detail & alasan: [`docs/hardware/power.md`](hardware/power.md).

### 7.4 Akustik
Mono: 1 driver 4 Ω + passive radiator (dari speaker BT bekas atau beli). Box tertutup rapat; ukuran dihitung dari parameter T/S driver final (contoh DMA45-4 → ~150 mL untuk Qtc ≈ 0,707).

## 8. Roadmap

| Fase | Isi | Tanda selesai | Milestone carousel |
|---|---|---|---|
| **0** | Persiapan: komponen, repo, PRD | Repo & PRD ada di GitHub | Ep 1–2 |
| **1** | Rakit breadboard + flash firmware siap pakai xiaozhi + server resmi xiaozhi.me | Bisa ngobrol dengan AI | Ep 5–6 |
| **2** | Server sendiri (Docker) + Whisper + LLM + TTS Indonesia; build firmware sendiri (SH1106) | Fasih B. Indonesia, server milik sendiri | Ep 7 |
| **3** | Modifikasi firmware: ring LED, touch, UI OLED, multi-SSID | Tampilan & kontrol versi sendiri | Ep 8 |
| **4** | Plugin: musik (YouTube, Spotify) + IoT (MQTT, HA/MCP) | "Putar lagu X", "nyalakan lampu" jalan | Ep 10–11 |
| **5** | Daya portable + box akustik + casing 3D | Unit portable utuh | Ep 3–4, 9 |
| **6** | Finishing, uji harian, rilis open-source | Rilis v1.0 di GitHub | Ep 12 |
| **7** | Rakitan kedua untuk tutorial YouTube (jalur C) | Video tutorial tayang | Tutorial |
| Nanti | Bluetooth out, PCB custom, DSP/EQ hardware, kit & produk | — | — |

Progres harian dicatat di [`docs/devlog.md`](devlog.md).

## 9. Rencana konten

- **Judul seri:** "Bikin Speaker AI dari Nol" — hardware rakit sendiri, firmware open-source yang dimodifikasi (atribusi xiaozhi disebut).
- **Gaya:** *daily tech vlog* / build in public — posting apa adanya setiap ada progres, konsisten, minim usaha, kegagalan ikut ditampilkan.

### 9.1 Tiga jalur konten

| Jalur | Kapan | Platform | Usaha |
|---|---|---|---|
| **A. Daily build vlog** ("Hari ke-N") | Setiap sesi kerja | IG Reels → YouTube Shorts → TikTok (9:16, 30–60 dtk) | Minim — konten utama |
| **B. Carousel milestone** | Saat milestone tercapai | IG feed → Threads (4:5, 1080×1350) | Sedang — ilmu & rangkuman |
| **C. Tutorial YouTube** | Setelah v1 jadi | YouTube (16:9, panjang) | Tinggi — dari **rakitan kedua** yang mulus |

- "Hari ke-N" dihitung **per sesi kerja**, bukan per tanggal kalender (tidak wajib upload tiap hari).
- Tutorial tidak direkam dari rakitan pertama (banyak jalan buntu). Rakitan kedua sekaligus menguji bahwa panduan bisa diikuti orang lain (bekal open-source & kit) dan menghasilkan unit kedua.
- Footage terbaik dari jalur A dipakai ulang sebagai B-roll jalur B & C.

### 9.2 Produksi
- **HP 1 (tripod):** tampak atas meja, horizontal 4K, posisi tetap, rekam terus tiap sesi.
- **HP 2 (holder meja):** close-up 30–45° atau wajah, vertikal 9:16, dipindah hanya untuk momen kunci.
- **Laptop:** rekam layar (Win+Alt+R / OBS), zoom 125–150 %.
- Tepuk tangan di awal (sinkron) dan di momen penting (penanda).
- Edit: CapCut, auto-caption, musik dari pustaka platform.
- **Privasi sebelum posting:** jangan tampilkan password WiFi, API key, `.config.yaml`, email pribadi.
- Detail lengkap: [`content/plan.md`](../content/plan.md).

### 9.3 Vlog awal (sebelum merakit)
| Hari | Isi |
|---|---|
| 1 | Persiapan: komponen & alat disusun rapi di meja (knolling, tampak atas) |
| 2 | Setup proyek dengan bantuan Claude: PRD, folder, GitHub (rekam layar) |

### 9.4 Milestone carousel (jalur B)

| Ep | Topik | Hook Reels | Isi carousel |
|---|---|---|---|
| 1 | Kenapa bikin ini | "Google Nest Mini, tapi rakit sendiri?" | Target fitur + arsitektur |
| 2 | Belanja komponen | Unboxing semua komponen | Daftar komponen + harga |
| 3 | Bongkar speaker BT rusak | "Speaker rusak ini jangan dibuang" | Cara uji driver & passive radiator |
| 4 | Desain power | Spot weld baterai | IP2312, BMS, MT3608 + tips aman |
| 5 | Mic + amp pertama | Bunyi pertama dari breadboard | Wiring I2S |
| 6 | Flash firmware | Ngobrol pertama dengan AI | Alur suara → server → jawaban |
| 7 | Bisa Bahasa Indonesia | "Akhirnya ngerti bahasa kita" | Whisper, LLM, TTS + biaya |
| 8 | OLED + touch + saklar mic | Wajah & ekspresi di OLED | UI + kenapa saklar mic fisik |
| 9 | Desain box + 3D print | Timelapse cetak casing | Hitungan box & passive radiator |
| 10 | Kontrol IoT | "Nyalakan lampu" pakai suara | MCP/MQTT ke modul |
| 11 | Putar musik | Minta lagu ke speaker | Cara kerja streaming |
| 12 | Reveal final | Adu suara vs Nest Mini | Rekap, total biaya, pelajaran |

Detail & template episode: [`content/plan.md`](../content/plan.md).

## 10. Biaya (perkiraan)

- **Hardware prototipe:** < Rp1 juta (sebagian besar sudah dibeli).
- **Bulanan:** STT Groq ≈ gratis; LLM Flash ≈ Rp20–40 rb; TTS Edge gratis; server di rumah (listrik). Target < Rp50 rb/bulan.

## 11. Risiko

| Risiko | Dampak | Mitigasi |
|---|---|---|
| YouTube tidak punya API resmi; integrasi komunitas sering rusak | Musik YouTube mati sewaktu-waktu | Jadikan plugin terpisah, siapkan alternatif (Spotify, radio, file lokal) |
| Spotify Connect butuh Premium | Fitur terbatas | Opsional |
| Free tier API (Gemini dll) berubah | Biaya naik / limit | LLM bisa diganti (API format OpenAI) |
| Server resmi xiaozhi.me bukan untuk komersial | Tidak bisa dipakai produk | Hanya untuk tes fase 1; fase 2 server sendiri |
| Tanpa AEC hardware (INMP441 + MAX98357) | Sulit menyela saat speaker bicara/musik | Terima di MVP; upgrade ke codec ES8311+ES7210 / mic array nanti |
| Modul MT3608 murah (trimpot, induktor jenuh) | Overvoltage / brownout | Resistor tetap, induktor Isat ≥ 3 A, kapasitor bulk |
| ESP32-S3 tanpa BT Classic | F7 tidak bisa di S3 | Ditunda; co-processor ESP32 / ESP32-S31 |
| Jual perangkat nirkabel di Indonesia | Perlu sertifikasi Komdigi (dulu SDPPI/Postel) | Cek regulasi sebelum produksi |

## 12. Keputusan yang sudah diambil

Lihat [`docs/decisions/`](decisions/).
1. Firmware base xiaozhi-esp32 (bukan ESPHome/Home Assistant)
2. Server AI self-hosted (xiaozhi-esp32-server)
3. Boost MT3608 dengan perbaikan modul
4. Bluetooth ke speaker lain ditunda
5. Hardware dari nol, firmware dari base open-source yang dimodifikasi
6. Lisensi MIT
7. Konten: daily build vlog + carousel milestone; tutorial YouTube dari rakitan kedua

## 13. Pertanyaan terbuka

- [ ] Nama/persona asisten (wake word custom?) — juga untuk branding konten
- [x] ~~Gaya konten~~ → daily build vlog, upload tiap ada progres (keputusan 7)
- [ ] Tampil muka atau tidak; voice-over atau teks saja
- [ ] Server di mana: PC rumah, STB, atau VPS
- [ ] LLM utama (Gemini Flash vs alternatif) & perlu mode "thinking"?
- [ ] Driver final: speaker 4 Ω yang ada vs Dayton DMA45-4 + passive radiator
- [ ] 1S2P atau 1S4P (ukuran casing vs daya tahan)
