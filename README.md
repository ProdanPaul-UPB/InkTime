# InkTime — Open Source Smartwatch

InkTime is a low-power, open-source smartwatch built around the Nordic nRF52840 SoC and a 1.54" e-paper display. The goal was to design a device that can run for weeks on a single charge while still being useful as a daily wearable — no constant charging, no always-on backlight draining the battery.

This repo contains the full hardware design: schematic, PCB layout, manufacturing files, and the complete 3D assembly.

---

## Block Diagram

```
                        ┌─────────────────────────────────────────┐
                        │            nRF52840 (U1)                │
  USB-C (J4) ──► ESD ──►│ USB D+/D-          SPI ──────────────► │──► EPD Display (J1)
       │        (D3)    │                                         │    (Waveshare 1.54")
       │                │ I2C ──────────────────────────────────► │──► BMA423 IMU (IC3)
       ▼                │                   ├──────────────────► │──► MAX17048 Fuel Gauge (U3)
  BQ25180 (IC1) ───────►│ VBUS sense         └──────────────────► │──► DRV2605 Haptic (IC2)
  LiPo Charger          │                                         │
       │                │ GPIO ──────────────────────────────────►│──► SW_UP / SW_ENT / SW_DN
       ▼                │                                         │
  AKYGA LP502030        │ RF ────────────────────────────────────►│──► Matching Network
  3.7V / 250mAh         │                                         │    └──► ANT1 (2.4GHz)
       │                │ SWD ───────────────────────────────────►│──► TC2030-IDC (J2)
       ▼                └─────────────────────────────────────────┘
  RT6160A (IC9)
  DC-DC → 3.3V ──────────────────────────────────────────────────► All 3.3V rails
```

---

## Bill of Materials

| Ref | Component | Value / Part | Package | Datasheet | JLC Parts |
|-----|-----------|--------------|---------|-----------|-----------|
| U1 | Nordic nRF52840 | nRF52840-QIAA | QFN-73 | [Datasheet](https://infocenter.nordicsemi.com/pdf/nRF52840_PS_v1.7.pdf) | [JLC](https://jlcpcb.com/partdetail/NordicSemiconductor-nRF52840QIAA/C190794) |
| IC1 | TI BQ25180 | LiPo Charger | DSBGA-9 | [Datasheet](https://www.ti.com/lit/ds/symlink/bq25180.pdf) | [JLC](https://jlcpcb.com/parts/componentSearch?searchTxt=BQ25180) |
| IC9 | RT6160A | DC-DC Buck 3.3V | WSON-6 | [Datasheet](https://www.richtek.com/assets/product_file/RT6160A/DS6160A-00.pdf) | [JLC](https://jlcpcb.com/parts/componentSearch?searchTxt=RT6160A) |
| U3 | MAX17048G+T10 | Fuel Gauge | SOT-23-6 | [Datasheet](https://www.analog.com/media/en/technical-documentation/data-sheets/MAX17048-MAX17049.pdf) | [JLC](https://jlcpcb.com/parts/componentSearch?searchTxt=MAX17048) |
| IC3 | BMA423 | Accelerometer | LGA-12 | [Datasheet](https://www.bosch-sensortec.com/media/boschsensortec/downloads/datasheets/bst-bma423-ds000.pdf) | [JLC](https://jlcpcb.com/parts/componentSearch?searchTxt=BMA423) |
| IC2 | DRV2605YZFR | Haptic Driver | DSBGA-12 | [Datasheet](https://www.ti.com/lit/ds/symlink/drv2605.pdf) | [JLC](https://jlcpcb.com/parts/componentSearch?searchTxt=DRV2605) |
| D3 | USBLC6-2SC6Y | USB ESD | SOT-363 | [Datasheet](https://www.st.com/resource/en/datasheet/usblc6-2.pdf) | [JLC](https://jlcpcb.com/parts/componentSearch?searchTxt=USBLC6-2SC6Y) |
| J4 | KH-TYPE-C-16P | USB-C Receptacle | SMD | — | [JLC](https://jlcpcb.com/parts/componentSearch?searchTxt=TYPE-C-16P) |
| J1 | Molex 503480-2400 | 24-pin FPC | SMD | [Datasheet](https://www.molex.com/en-us/products/part-detail/503480-2400) | [JLC](https://jlcpcb.com/parts/componentSearch?searchTxt=503480-2400) |
| J2 | TC2030-IDC | SWD Connector | THT | [Datasheet](https://www.tag-connect.com/product/tc2030-idc-nl) | — |
| ANT1 | 2450AT18B100E | 2.4GHz Antenna | SMD | [Datasheet](https://www.johansontechnology.com/datasheets/2450AT18B100E/2450AT18B100E.pdf) | [JLC](https://jlcpcb.com/parts/componentSearch?searchTxt=2450AT18B100E) |
| X1 | 32MHz Crystal | HFXO | SMD | — | [JLC](https://jlcpcb.com/parts/componentSearch?searchTxt=32MHz+crystal) |
| X2 | 32.768kHz Crystal | LFXO | SMD | — | [JLC](https://jlcpcb.com/parts/componentSearch?searchTxt=32.768kHz+crystal) |
| SW1-3 | EVP-AKE31A | Tactile Button | SMD | [Datasheet](https://industrial.panasonic.com/cdbs/www-data/pdf/ATV0000/ATV0000CE5.pdf) | [JLC](https://jlcpcb.com/parts/componentSearch?searchTxt=EVP-AKE31A) |
| — | AKYGA LP502030 | 3.7V / 250mAh LiPo | — | [Datasheet](https://www.tme.eu/Document/b9e12bf26ad0ba929a22ab5d58f022cd/AKY0106.pdf) | External |
| — | Waveshare WSH-12561 | 1.54" EPD Display | FPC-24 | [Datasheet](https://www.tme.eu/Document/0ca57a8ffbcd57b5bca53252eb9d6ec3/WSH-12561.pdf) | External |
| — | DFRobot FIT0774 | Vibration Motor | Coin 10×2.7mm | [Product page](https://www.tme.eu/ro/details/df-fit0774/motoare-dc/dfrobot/fit0774/) | External |
| Q3 | DMG2305UX | P-MOSFET | SOT-523 | [Datasheet](https://www.diodes.com/assets/Datasheets/DMG2305UX.pdf) | [JLC](https://jlcpcb.com/parts/componentSearch?searchTxt=DMG2305UX) |
| Q4 | SI1308EDL | N-MOSFET | SC-70 | [Datasheet](https://www.vishay.com/docs/73217/si1308edl.pdf) | [JLC](https://jlcpcb.com/parts/componentSearch?searchTxt=SI1308EDL) |
| L5 | 68µH Inductor | EPD Boost | 0402 | — | [JLC](https://jlcpcb.com/parts/componentSearch?searchTxt=68uH+0402) |
| L1 | 3.9nH Inductor | RF Matching | 0402 | — | [JLC](https://jlcpcb.com/parts/componentSearch?searchTxt=3.9nH+0402) |
| L2 | 15nH Inductor | RF Matching | 0402 | — | [JLC](https://jlcpcb.com/parts/componentSearch?searchTxt=15nH+0402) |
| L7 | 4.7µH Inductor | DC-DC Switching | 0402 | — | [JLC](https://jlcpcb.com/parts/componentSearch?searchTxt=4.7uH+0402) |
| R9 | 10kΩ | TS/MR Pull-down | 0201 | — | [JLC](https://jlcpcb.com/parts/componentSearch?searchTxt=10k+0201) |
| Various C | 100nF / 1µF / 10µF / 22µF | Decoupling | 0201 / 0402 | — | [JLC](https://jlcpcb.com/parts/componentSearch?searchTxt=100nF+0201) |

---

## Hardware Description

### Power Path

Power enters through the USB-C connector (J4). The BQ25180 charger (IC1) handles incoming VBUS and takes care of charging the LiPo battery safely — it supports up to 1A charge current with I²C-configurable parameters. The RT6160A (IC9) is a synchronous buck DC-DC converter that takes the battery voltage (3.0–4.2V) and steps it down to a clean 3.3V rail used by everything else on the board. The MAX17048 fuel gauge (U3) sits on the I²C bus and reports state of charge to the MCU so the watch can display battery percentage without the MCU having to do any ADC math itself.

The battery (AKYGA LP502030) is soldered directly to two test pads on the board rather than using a JST connector — this saves ~2mm of vertical height which matters a lot inside a watch case.

### Microcontroller

The nRF52840 (U1) is the brain of the device. It's a Cortex-M4F running at 64MHz with built-in Bluetooth 5.0 and a hardware USB controller. We used the QFN-73 package which has a 5×5mm exposed thermal pad underneath — there are thermal vias going through to the bottom GND plane to help with heat dissipation. The chip runs off the 3.3V rail and has 13 decoupling capacitors placed in a ring around its pads, as close to the power pins as the 2.54mm grid allows.

### Display Interface

The 1.54" e-paper display (Waveshare WSH-12561) connects through a 24-pin FPC connector (J1, Molex 503480-2400) on the left edge of the board. The EPD uses a 4-wire SPI interface plus three control lines (DC, RST, BUSY). The display only draws current during refresh — in static mode it consumes essentially nothing, which is the main reason e-paper was chosen for a watch that's supposed to last weeks.

Driving an EPD requires specific voltages that 3.3V alone can't provide. The EPD drive circuit uses a boost converter (Q4 N-MOSFET + L5 68µH inductor + D2 Schottky rectifier) to generate the higher voltages the display panel needs for VCOM and VDD. Q3 is a P-MOSFET power switch that the MCU uses to completely cut power to the display when it doesn't need it.

### Inertial Measurement Unit

The BMA423 (IC3) is a 6-axis IMU that handles step counting, activity recognition, and wrist-tilt detection (to wake the display when the user raises their wrist). It communicates over I²C and has its own interrupt output connected to a GPIO on the nRF52840 so it can wake the MCU from deep sleep without the MCU polling.

### Haptic Feedback

The DRV2605 (IC2) is a haptic driver that can drive both ERM (eccentric rotating mass) and LRA (linear resonant actuator) motors. It connects over I²C and has a library of pre-programmed waveforms built into its ROM, so the MCU just writes an effect number and the chip handles the timing. The FIT0774 vibration motor is a 10×2.7mm coin ERM motor that sits in the corner of the case behind the PCB.

### Bluetooth / RF

The nRF52840's built-in 2.4GHz radio connects to a chip antenna (Johanson 2450AT18B100E) through a pi-type matching network made of two series inductors (L1 3.9nH, L2 15nH) and two shunt capacitors (C3, C5 each 1pF). The antenna is placed at the edge of the PCB with a keepout zone below it — no copper planes or signal traces run under the antenna. Via stitching connects the top and bottom GND planes around the RF area to prevent ground loops from affecting the antenna's radiation pattern.

### USB and ESD Protection

The USBLC6-2SC6Y (D3) is a dual-channel TVS diode array placed between the USB-C connector and the nRF52840's USB pins. It clamps any ESD spikes from the connector before they can reach the MCU. The two CC pull-down resistors (5.1kΩ each) on CC1 and CC2 tell the USB host that this is a UFP (upstream facing port) device drawing up to 500mA.

### Debug Interface

The TC2030-IDC (J2) is a Tag-Connect 2×5 pogo-pin footprint for SWD debugging. It brings out SWDIO, SWDCLK, SWO, and RESET so a J-Link or similar debugger can program and debug the nRF52840 without needing a soldered header. Several test pads scattered around the board (labeled in silkscreen) expose the main power rails and I²C/SPI signals for probing during bring-up.

---

## nRF52840 Pin Mapping

| Pin | Signal | Connected To | Notes |
|-----|--------|-------------|-------|
| P0.06 | SDA | BMA423, MAX17048, DRV2605, BQ25180 | Shared I²C data |
| P0.08 | SCL | BMA423, MAX17048, DRV2605, BQ25180 | Shared I²C clock |
| P0.11 | MOSI | EPD Display (J1) | SPI data to display |
| P0.12 | MISO | EPD Display (J1) | SPI data from display |
| P0.13 | SCK | EPD Display (J1) | SPI clock |
| P0.14 | CS_EPD | EPD Display (J1) | SPI chip select |
| P0.15 | DC_EPD | EPD Display (J1) | Data/Command select |
| P0.16 | RST_EPD | EPD Display (J1) | Display reset |
| P0.17 | BUSY_EPD | EPD Display (J1) | Display busy indicator |
| P0.02 | SW_UP | SW1 (Up button) | Active low, internal pull-up |
| P0.03 | SW_ENT | SW2 (Enter button) | Active low, internal pull-up |
| P0.04 | SW_DN | SW3 (Down button) | Active low, internal pull-up |
| P0.29 | INT_IMU | BMA423 (IC3) | Interrupt from IMU |
| P0.30 | EN_EPD | Q3 gate | EPD power switch enable |
| P0.31 | EN_HAP | DRV2605 IN/TRIG | Haptic trigger signal |
| P0.20 | D+ | USBLC6 → USB-C | USB data positive |
| P0.22 | D− | USBLC6 → USB-C | USB data negative |
| SWDIO | SWDIO | TC2030-IDC (J2) | SWD debug interface |
| SWDCLK | SWDCLK | TC2030-IDC (J2) | SWD debug clock |
| P0.18 | RESET | TC2030-IDC (J2) | Hardware reset |
| XC1/XC2 | HFXO | X1 (32MHz) | High-frequency crystal oscillator |
| XL1/XL2 | LFXO | X2 (32.768kHz) | Low-frequency crystal for RTC |

The I²C bus is shared between four ICs — the BMA423, MAX17048, DRV2605, and BQ25180. Each has a different I²C address so there are no conflicts. SCL runs at 400kHz (fast mode).

---

## Power Consumption (estimated)

| Mode | Current draw |
|------|-------------|
| Deep sleep (display static, BLE off) | ~10–15 µA |
| Active (BLE advertising, IMU polling) | ~3–5 mA |
| Display refresh (EPD update) | ~20–30 mA for ~2s |
| USB charging | up to 250mA (BQ25180 configured) |

With the 250mAh battery and an estimated average current of ~0.5mA (mostly in deep sleep, occasional display updates and BLE), the target runtime is around 3–4 weeks between charges.

---

## PCB Details

- **Board dimensions:** 45.72mm × 34.29mm
- **Layers:** 2 (Top + Bottom)
- **Thickness:** 1.0mm
- **Surface finish:** HASL or ENIG
- **Min trace width:** 0.15mm (signals), 0.3mm (power)
- **Via drill:** 0.3mm min
- **GND planes:** Both TOP and BOTTOM layers, connected via stitching vias
- **Stackup:** Signal + GND pour (TOP) / GND pour + Signal (BOTTOM)
- **Antenna keepout:** No copper or signals under ANT1

All components are on the TOP layer only.

---

## Known Issues and Design Decisions

- **Board outline clearance errors:** The three tactile buttons (SW1–SW3) and the USB-C connector are intentionally positioned so they protrude into the board edge clearance zone. This is by design — they need to align with cutouts in the physical case and this placement was confirmed in the reference `.fbrd` layout from the course. These DRC errors are accepted and documented here.

- **Drill size errors:** Some via drill sizes fall slightly below the JLCPCB minimum due to the dense BGA escape routing under U1. These were the smallest vias needed to escape the MCU pads while still leaving clearance for adjacent traces. A PCB house that supports 0.2mm drills (like PCBWay) can manufacture these without issue. For JLCPCB specifically, these can be bumped to 0.3mm at the cost of routing one or two signals to alternate paths.

- **Battery connection:** The battery is soldered directly to TP_VBAT and TP_GND test pads instead of using the JST connector. This was intentional to reduce the height of the assembly so it fits inside the 1.0mm-thick PCB + battery stack constraint of the inktime_case enclosure.

- **EPD drive circuit:** The boost converter was designed around the voltage requirements in the WSH-12561 datasheet. The VCOM voltage is set via SJ1 — the solder jumper selects between direct GND and a resistor-divided reference, allowing adjustment during bring-up if the display contrast isn't right with the default value.

---

## Repository Structure

```
Hardware/
├── inktime.sch          — Schematic (Fusion 360 / EAGLE format)
├── inktime.brd          — PCB board file
└── inktime_schematic.pdf
Manufacturing/
├── gerbers.zip
├── inktime.bom
└── inktime.cpl
Mechanical/
├── inktime_assembly_exploded.step
└── inktime_complete.f3z
Images/
└── (PCB renders and assembly photos)
LICENSE
README.md
```