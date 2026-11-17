# Pinout — ESP32-S3-N16R8 DevKit

Mengikuti board profile `bread-compact-wifi` di xiaozhi-esp32 (`main/boards/bread-compact-wifi/config.h`), supaya firmware siap pakai langsung jalan. Baris bertanda **(mod)** adalah tambahan kita yang belum ada di config asli.

| Modul | Pin modul | GPIO | Daya | Catatan |
|---|---|---|---|---|
| INMP441 | WS | 4 | 3V3 | |
| INMP441 | SCK | 5 | | |
| INMP441 | SD | 6 | | L/R → GND (kanal kiri) |
| MAX98357A | DIN | 7 | 5V | GAIN & SD dibiarkan (default) |
| MAX98357A | BCLK | 15 | | |
| MAX98357A | LRC | 16 | | |
| OLED SH1106 | SDA | 41 | 3V3 | Pilih `OLED_SH1106_128X64` di menuconfig |
| OLED SH1106 | SCL | 42 | | |
| Tombol BOOT | — | 0 | — | Onboard |
| Touch 1 (TTP223) | OUT | 47 | 3V3 | "Talk/interrupt". Jumper pad **A** → aktif LOW |
| Touch 2 (TTP223) | OUT | 40 | 3V3 | Sementara Vol+. (mod) tap = Vol+, tahan = Vol− |
| (cadangan) Vol− | — | 39 | — | Ada di config asli |
| WS2812B ring 16 | DIN | 21 **(mod)** | 5V | Resistor seri 330 Ω, elco 470–1000 µF di 5V, brightness ≤ 20 % |
| Lampu demo IoT | — | 18 | — | `LAMP_GPIO` di config asli (demo MCP) |
| LED onboard | — | 48 | — | Dipakai config asli, jangan dipakai lagi |
| ADC baterai | — | TBD **(mod)** | — | Pembagi 100k/100k + 100 nF (fase 5) |

## Pin yang TIDAK boleh dipakai (N16R8)
- GPIO 26–32: flash SPI
- GPIO 35–37: octal PSRAM
- GPIO 19 / 20: USB D− / D+
- GPIO 0, 3, 45, 46: strapping (0 = BOOT)

## Catatan
- TTP223 mengkalibrasi saat power-on — jangan disentuh ketika menyalakan.
- WS2812 dengan data 3,3 V dan suplai 5 V itu marginal (VIH = 0,7 × VDD). Kalau warna kacau: beri daya ring lewat 1 dioda (~4,3 V) atau pakai level shifter 74AHCT125.
- Saat masih ditenagai USB laptop, batasi brightness ring (16 LED putih penuh ≈ 1 A).
