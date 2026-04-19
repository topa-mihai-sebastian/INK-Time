# InkTime Smartwatch

## Overview

InkTime este un smartwatch low-power, open-source, optimizat pentru autonomie ridicată (≥30 zile), folosind un display e-paper și o arhitectură hardware eficientă energetic.

Sistemul este construit în jurul microcontrollerului nRF52840 și integrează:
- afișare e-paper
- notificări BLE
- accelerometru pentru pași
- feedback haptic
- management eficient al energiei

---

## Block Diagram

```
           [ USB Type-C ]                              [ LiPo Battery ]
                 |                                            |
                 v                                            |
          +---------------+                                   |
          |   BQ25180     |<----------------------------------+
          |   Charger     |                                   |
          +---------------+                                   v
                 | VSYS                             +---------------+
                 v                                  |  MAX17048     |
          +---------------+                         | Fuel Gauge    |
          |   RT6160      |                         +---------------+
          | Buck-Boost    |                                 |
          +---------------+                                 |
                 | 3.3V                                     |
                 |                                          |
        +------------------- nRF52840 MCU ------------------+
        |                                                   |
        | I2C ---> BMA421                                   |
        | I2C ---> DRV2605L ---> Motor                      |
        | I2C ---> MAX17048                                 |
        | I2C ---> BQ25180, RT6160                          |
        |                                                   |
        | SPI ---> E-paper Display                          |
        | GPIO ---> Buttons                                 |
        | GPIO ---> PFET (Display Power)                    |
        +---------------------------------------------------+
```

---

## Bill of Materials (BOM)

| Funcție | Componentă | Package | JLC Code | Datasheet |
|--------|-----------|--------|----------|----------|
| MCU | nRF52840-QIAA-R | aQFN73 | C190794 | Nordic |
| Charger | BQ25180YBGR | DSBGA-8 | C3682423 | TI |
| Regulator | RT6160AWSC | WLCSP-15 | C7065276 | Richtek |
| Fuel Gauge | MAX17048G+T10 | DFN-8 | C2682616 | Maxim |
| Accelerometru | BMA421 | LGA-12 | C5242966 | Bosch |
| Haptic Driver | DRV2605LDGSR | VSSOP-10 | C527464 | TI |
| Motor | LCM1027B3605F | Wire | C7528806 | ERM |
| PFET | SI2301CDS | SOT-23 | C10487 | Vishay |
| USB-C | KH-TYPE-C-16P | SMD | - | Generic |
| Debug | TC2030 | Footprint | - | Tag-Connect |

---

## Hardware Functionality

### Power
- USB-C 5V input
- BQ25180 charger cu power-path
- RT6160 pentru 3.3V
- MAX17048 pentru monitorizare baterie

### MCU
- nRF52840 (BLE + control sistem)
- RTC pentru actualizare timp

### Display
- E-paper 1.54”
- update periodic
- alimentare controlată prin PFET

### Senzori și Haptic
- BMA421 pentru pași
- DRV2605L pentru vibrații

### Input
- 3 butoane (active low)

---

## nRF52840 Pin Mapping

### I2C
- P0.06 SDA
- P0.07 SCL

### SPI
- P0.02 SCK
- P0.03 MOSI
- P0.05 CS
- P0.15 DC
- P0.16 RST
- P0.17 BUSY

### GPIO
- P1.01 PFET
- P0.12 Haptic EN

### Interrupturi
- P0.08 IMU INT1
- P1.08 IMU INT2
- P0.10 Fuel gauge ALERT
- P0.11 Charger INT

### Butoane
- P0.13 UP
- P0.14 DOWN
- P1.00 ENTER

---

## Design Decisions

- componente doar pe TOP layer
- plane de GND pe TOP și BOTTOM
- via stitching în zona RF
- decuplare aproape de pini
- keepout sub antenă
- baterie fără conector JST

---

## DRC și Verificări

- ERC verificat
- DRC rulat
- excepții:
  - Only INPUT pins on NET ID
  - overlap mecanic USB/butoane

---

## Concluzie

Design-ul este optimizat pentru consum redus, integrare compactă și producție în masă.
