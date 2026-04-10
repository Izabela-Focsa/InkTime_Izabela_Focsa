# InkTime Smartwatch - Open Source Wearable (EVT Stage)

## Proiect Overview
InkTime este un start-up care dezvoltă un smartwatch open-source și accesibil, bazat pe microcontrolerul **nRF52840**. Acest repository conține fișierele de proiectare hardware, fișierele pentru producție și modelarea mecanică necesare etapei de Engineering Validation (EVT).

---

## 🏗️ Hardware Architecture

### Diagrama Bloc
Dispozitivul este construit în jurul SoC-ului nRF52840, gestionând comunicarea BLE, afișajul E-Paper și senzorii periferici prin interfețe digitale dedicate.



[Image of nRF52840 block diagram]


Dispozitivul integrează următoarele module principale:
* **MCU:** Nordic nRF52840 (Bluetooth LE, procesare eficientă).
* **Display:** E-Paper Display (conectat via SPI).
* **Power Management:** Încărcare prin USB (VBUS/VUSB) via BQ25180 și regulator Buck-Boost RT6160A pentru 3.3V stabil.
* **Senzori/Actuatori:** Accelerometru BMA423, Haptic Driver DRV2605, butoane de control.

### Pinout Assignment (nRF52840)
| Componentă | Pini nRF52840 | Interfață | Justificare |
| :--- | :--- | :--- | :--- |
| **Display (E-Paper)** | P0.28 (SCK), P0.29 (MOSI), P0.30 (CS), P0.31 (D/C), P0.02 (RST), P0.03 (BUSY) | SPI | [cite_start]Protocol de mare viteză necesar pentru transferul buffer-ului de imagine[cite: 1]. |
| **Butoane** | P0.08 (UP), P0.11 (ENTER), P0.12 (DOWN) | GPIO (Input) | [cite_start]Configurați cu pull-up intern; permit navigarea în meniul ceasului[cite: 1]. |
| **Vibrator (Haptic)** | P0.16 (HAPTIC_INT) | PWM/GPIO | [cite_start]Conectat la pinul IN/TRIG al driverului DRV2605 pentru controlul vibrațiilor[cite: 1]. |
| **I2C Senzori** | P0.14 (SDA), P0.15 (SCL) | I2C | [cite_start]Bus comun utilizat pentru IMU (BMA423) și Fuel Gauge (MAX17048)[cite: 1]. |

---

## 📋 Bill of Materials (BOM)
Componentele critice au fost selectate din biblioteca JLC Parts pentru a facilita producția în masă.

| Designator | Componentă | Capsulă | Descriere |
| :--- | :--- | :--- | :--- |
| **U1** | nRF52840 | aQFN73 | [cite_start]Microcontroler principal BLE[cite: 1]. |
| **U2** | DRV2605L | VSSOP-10 | [cite_start]Haptic Driver[cite: 1]. |
| **U3** | MAX17048 | TDFN-8 | [cite_start]Fuel Gauge (Monitorizare baterie)[cite: 1]. |
| **IC1** | BQ25180 | WLCSP-8 | [cite_start]LiPo Charger Management[cite: 1]. |
| **IC2** | BMA423 | LGA-12 | [cite_start]Accelerometru 3-axe[cite: 1]. |
| **IC3** | RT6160A | WLCSP-15 | [cite_start]Convertor DC-DC Buck-Boost 3.3V[cite: 1]. |
| **ANT1** | 2450AT18B100E | 1206 (SMD) | [cite_start]Antenă ceramică chip[cite: 1]. |

---

## ⚡ Detalii Tehnice & Constrângeri Design
* **Reguli de Rutare:** Trasee de putere (VCC, 3V3) de **0.3mm**, semnale de date de **0.15mm**.
* **EMI/RF:** Plan de masă continuu pe Bottom, decupaj sub antenă pentru a evita atenuarea semnalului BLE.
* **Decuplare:** Condensatoarele de 100nF (capsulă 0201) sunt plasate la <1mm de pinii de alimentare ai IC-urilor.
* **Mecanică:** Design compact, componente montate exclusiv pe layer-ul **TOP**.
* **Consum:** Bateria LiPo este monitorizată prin Fuel Gauge pentru a optimiza modurile de sleep ale nRF52840.

---

## 📸 Randări și Design Log
To be added

## 📄 Licență
Acest proiect este licențiat sub [GNU General Public License v3.0](Licence.txt).
