# Bill of Materials

Status: ✅ sudah ada · 🛒 dibeli/dalam pengiriman · ⬜ belum

## Inti
| Komponen | Qty | Status | Catatan |
|---|---|---|---|
| ESP32-S3-N16R8 DevKit | 1 | ✅ | |
| INMP441 mic I2S | 2 | ✅ | 1 spare / untuk 2-mic nanti |
| MAX98357A amp I2S | 3 | ✅ | 2 spare |
| Speaker 4 Ω 5–10 W neodymium | 2 | ✅ | Klaim "10 Hz–120 kHz" = marketing. Ukur DCR (3,2–3,8 Ω = OK) |
| OLED 1,3" SH1106 I2C | 1 | ✅ | |
| Ring WS2812B 16 LED 5050 | 1 | ✅ | |
| Sensor touch TTP223 | 2 | ✅ | |

## Daya
| Komponen | Qty | Status | Catatan |
|---|---|---|---|
| Modul charger IP2312 | 2 | ✅ | Pastikan varian **4,2 V** |
| Modul boost MT3608 | 2 | ✅ | Ganti trimpot → resistor tetap |
| BMS 1S | 3 | ✅ | Rating ≥ 3 A (lebih baik 5 A) |
| Baterai 18650 | 2 (atau 4) | ⬜ | Merek jelas, satu tipe; hindari klaim "9800 mAh" |
| Holder 18650 / nikel strip | — | ⬜ | Holder untuk prototipe |

## Pasif & lain-lain
| Komponen | Qty | Status | Untuk |
|---|---|---|---|
| Schottky SS34/SS54 | 2–3 | ⬜ | Load sharing |
| P-MOSFET SOT-23 (AO3401) + 100k | 2–3 | ⬜ | Load sharing |
| Resistor 75k + 10k | beberapa | ⬜ | Set MT3608 ≈ 5,1 V |
| Resistor 68k | 2 | ⬜ | RICHG IP2312 ≈ 2 A |
| Resistor 100k ×2 + 100 nF | beberapa | ⬜ | ADC baterai |
| Resistor 330 Ω | 2 | ⬜ | Data WS2812 |
| Elco 470–1000 µF 10 V low-ESR | 3–4 | ⬜ | VDD amp, output boost, ring LED |
| Kapasitor keramik 100 nF / 10 µF | beberapa | ⬜ | Decoupling |
| Saklar slide | 2 | ⬜ | Saklar mic fisik + power utama |
| Breadboard, jumper, perfboard, header, kabel silikon AWG 20–22, JST-PH/XH | — | ⬜ | Perakitan |

## Opsional / nanti
| Komponen | Untuk |
|---|---|
| Passive radiator | Bass (cek dulu dari speaker BT rusak) |
| Dayton Audio DMA45-4 + passive radiator pasangannya | Driver final kualitas lebih baik |
| Induktor Isat ≥ 3 A | Upgrade MT3608 |
| NTC baterai | Proteksi suhu (versi final) |
| ESP32 klasik (WROOM-32) | Bluetooth A2DP ke speaker lain (F7, ditunda) |
