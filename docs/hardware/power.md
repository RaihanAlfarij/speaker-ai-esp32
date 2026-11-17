# Desain Daya

```
USB-C 5V ──► IP2312 ──► BMS 1S ──► Baterai 1S2P (18650)
   │                                  │
   └──(Schottky)──┬──(P-MOSFET)───────┘   ← load sharing
                  ▼
           MT3608 boost 5,0–5,1 V
                  ├──► MAX98357A (VDD 5V)
                  ├──► ESP32-S3 devkit (pin 5V → LDO onboard)
                  └──► Ring WS2812 (5V)
```

## IP2312 (charger)
- Charger buck sinkron 5 V → Li-ion 1 sel, maks 3 A, efisiensi ±94 %.
- Arus charge: `ICHG = 135000 / RICHG`. **1S2P → 68 kΩ (≈ 2 A, ±1 A per sel)**. 1S4P → 3 A boleh.
- Pin NTC: kalau tidak dipakai, ikat ke GND lewat **51 kΩ** (jangan floating). Charger throttle > 43 °C, stop > 45 °C.
- IP2312 **tidak punya power path** dan punya timeout charge 24 jam → wajib load sharing (di bawah).

## Load sharing
Saat USB tercolok: sistem diberi daya dari VBUS lewat Schottky, jalur baterai diputus P-MOSFET (gate ditarik oleh VBUS). Tujuannya: charger bisa mendeteksi baterai penuh, dan baterai tidak terus "dicas sambil dipakai".

## MT3608 (boost)
- Batas arus switch internal 4 A (dijamin desain), cukup untuk puncak ±1,1 A di rail 5 V (amp ±0,75 A + ESP32 ±0,35 A).
- **Ganti trimpot dengan resistor tetap**: `VOUT = 0,6 × (1 + R1/R2)` → R1 = 75k, R2 = 10k ≈ 5,1 V. (MAX98357A maks 5,5 V.)
- Induktor modul murah cepat jenuh → kalau suara pecah/ESP32 reset saat bass, ganti induktor Isat ≥ 3 A.
- Elco 470–1000 µF low-ESR dekat VDD amp.
- Non-sinkron → efisiensi lebih rendah, perkiraan daya tahan turun ±10–15 %.
- Jauhkan dari mic; beri filter RC/LC pada 3V3 mic bila ada noise.

## Proteksi baterai
- BMS 1S rating ≥ 3 A.
- Monitor tegangan via ADC (pembagi 100k/100k), **cutoff software ±3,3 V**. Cutoff BMS (±2,4–2,5 V) hanya pengaman terakhir.
- Sel paralel: satu tipe/kapasitas, samakan tegangan sebelum disambung, **spot weld** (bukan solder).

## Estimasi daya tahan (kasar)
Rata-rata 1,5–2 W (ESP32 WiFi aktif + amp volume sedang + rugi boost):
- 1S2P (±18 Wh): **8–12 jam**
- 1S4P: 16–24 jam (casing jauh lebih besar)
