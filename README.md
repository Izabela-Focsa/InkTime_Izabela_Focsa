# InkTime Smartwatch - Dispozitiv Wearable Open Source (Stadiu EVT)

## 📌 Prezentare Generală

**InkTime** este un smartwatch open-source, proiectat în jurul microcontrolerului **Nordic nRF52840**, cu accent pe consum redus de energie, conectivitate Bluetooth Low Energy și modularitate hardware.

Acest repository conține:

* designul PCB (Fusion Electronics)
* fișierele pentru fabricație (Gerber, BOM, Pick & Place)
* designul mecanic (Fusion 360)
* integrarea sistemului pentru etapa EVT (Engineering Validation Test)

---

## 🧩 Diagrama Bloc a Sistemului

Arhitectura sistemului este centrată în jurul microcontrolerului nRF52840, care gestionează comunicația, afișajul și senzorii.

```
        +----------------------+
        |     Baterie LiPo     |
        +----------+-----------+
                   |
            +------v------+
            |  BQ25180    |  (Încărcare)
            +------+------+
                   |
            +------v------+
            |  RT6160A    |  (Regulator 3.3V)
            +------+------+
                   |
         +---------v---------+
         |    nRF52840 MCU   |
         | (BLE + Procesare) |
         +---+----+----+-----+
             |    |    |
        SPI  |    |    | I2C
             |    |    |
   +---------v+  |   +v-----------+
   | Display   |  |   | BMA423 IMU|
   | E-Paper   |  |   +------------+
   +----------+  |
                 |
             +---v---------+
             | MAX17048    |
             | Fuel Gauge  |
             +-------------+

             +-------------+
             | DRV2605L    |
             | Vibrații    |
             +-------------+

             +-------------+
             | Butoane     |
             +-------------+
```

---

## 🔌 Funcționalitatea Hardware (Detaliat)

### 🧠 Microcontroler – nRF52840

* ARM Cortex-M4F @ 64 MHz
* Bluetooth Low Energy 5.0 integrat
* Consum foarte redus în mod sleep

Roluri principale:

* procesare date senzori
* control display
* comunicare BLE

---

### 📺 Display – E-Paper

* Interfață: **SPI**
* Consum foarte mic
* Ideal pentru afișare permanentă

---

### ⚡ Managementul Energiei

#### 🔋 BQ25180

* gestionează încărcarea bateriei LiPo
* alimentare prin USB (VBUS)

#### 🔌 RT6160A

* convertor Buck-Boost
* furnizează **3.3V stabil**
* permite funcționarea chiar și la tensiuni scăzute ale bateriei

---

### 📊 Senzori și Actuatori

#### BMA423 (Accelerometru)

* interfață I2C
* detectare mișcare / pași

#### MAX17048 (Fuel Gauge)

* monitorizare nivel baterie
* consum foarte redus

#### DRV2605L (Driver Haptic)

* controlează motorul de vibrații
* suportă feedback haptic avansat

---

### 🎮 Input Utilizator

* 3 butoane: UP / ENTER / DOWN
* conectate ca GPIO cu rezistențe pull-up interne

---

## 🔗 Mapare Pini nRF52840

| Componentă          | Pini                                                                          | Interfață | Motiv                  |
| ------------------- | ----------------------------------------------------------------------------- | --------- | ---------------------- |
| **Display E-Paper** | P0.28 (SCK), P0.29 (MOSI), P0.30 (CS), P0.31 (D/C), P0.02 (RST), P0.03 (BUSY) | SPI       | Transfer rapid de date |
| **Butoane**         | P0.08, P0.11, P0.12                                                           | GPIO      | Input simplu           |
| **Haptic Driver**   | P0.16                                                                         | GPIO/PWM  | Control vibrații       |
| **Senzori I2C**     | P0.14 (SDA), P0.15 (SCL)                                                      | I2C       | Magistrală comună      |
| **Fuel Gauge**      | I2C                                                                           | I2C       | Monitorizare baterie   |

---

## 📦 Bill of Materials (BOM)

| Ref  | Componentă | Link JLCPCB                                             | Datasheet                                                                                      |
| ---- | ---------- | ------------------------------------------------------- | ---------------------------------------------------------------------------------------------- |
| U1   | nRF52840   | https://jlcpcb.com/partdetail/Nordic-nRF52840           | https://infocenter.nordicsemi.com/pdf/nRF52840_PS_v1.1.pdf                                     |
| U2   | DRV2605L   | https://jlcpcb.com/partdetail/TexasInstruments-DRV2605L | https://www.ti.com/lit/ds/symlink/drv2605l.pdf                                                 |
| U3   | MAX17048   | https://jlcpcb.com/partdetail/MaximIntegrated-MAX17048  | https://datasheets.maximintegrated.com/en/ds/MAX17048-MAX17049.pdf                             |
| IC1  | BQ25180    | https://jlcpcb.com/partdetail/TexasInstruments-BQ25180  | https://www.ti.com/lit/ds/symlink/bq25180.pdf                                                  |
| IC2  | BMA423     | https://jlcpcb.com/partdetail/Bosch-BMA423              | https://www.bosch-sensortec.com/media/boschsensortec/downloads/datasheets/bst-bma423-ds000.pdf |
| IC3  | RT6160A    | https://jlcpcb.com/partdetail/Richtek-RT6160A           | https://www.richtek.com/assets/product_file/RT6160A/DS6160A-00.pdf                             |
| ANT1 | Antenă BLE | https://jlcpcb.com                                      |                                                                          |

---

## ⚙️ Constrângeri și Decizii de Design

### PCB

* trasee alimentare: **0.3 mm**
* trasee semnal: **0.15 mm**
* PCB pe 2 straturi

### RF

* plan de masă continuu
* zonă fără cupru sub antenă

### Decuplare

* condensatori 100nF la <1mm de fiecare IC

### Mecanic

* toate componentele pe **TOP layer**
* design compact pentru carcasă

---

## 🔋 Estimare Consum Energetic

| Mod                   | Consum    |
| --------------------- | --------- |
| Activ (BLE + display) | ~15–20 mA |
| Idle                  | ~2–5 mA   |
| Sleep                 | <10 µA    |

Baterie: ~200–300 mAh
Autonomie estimată: **2–5 zile**

---

## 🖼️ Randări și Design

Randările dispozitivului, ale PCB-ului și integrarea în carcasă pot fi găsite în folderul dedicat din repository:

👉 [Images Folder](./Images)

---

## 🛠️ Fișiere pentru Fabricație

* Gerber
* Drill files
* Pick & Place (.csv)
* BOM (.csv)

---

## 📘 Design Log

* realizare schemă electronică
* rutare PCB completă
* verificare DRC
* integrare mecanică finală

---

## 📄 Licență

Proiectul este licențiat sub **GNU General Public License v3.0**.
