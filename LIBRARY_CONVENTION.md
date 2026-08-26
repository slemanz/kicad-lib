# Library Convention

Naming and organization standard for this KiCad library.
Every symbol, footprint and 3D model in this repository must follow it.

---

## 1. Repository layout

```
kicad-lib/
├── symbol/                 # one .kicad_sym per category
├── footprint/              # one .pretty directory per category
├── 3d/                     # STEP/WRL, flat, named after the footprint
└── LIBRARY_CONVENTION.md
```

Symbol library files and footprint directories use **the same category names**, so
`06_ics.kicad_sym` pairs with `footprint/06_ics.pretty/`.

The numeric prefix controls sort order in the KiCad chooser. It is part of the
nickname, do not renumber an existing category once parts reference it.

---

## 2. Categories

Twelve categories. Fine-grained families are expressed by the **part prefix**, not
by a new library.

| Library | Contents | Part prefixes |
|---|---|---|
| `01_resistors` | Fixed, arrays, shunts, thermistors, potentiometers | `RES`, `RES-A`, `RES-SH`, `NTC`, `PTC`, `POT` |
| `02_capacitors` | Ceramic, electrolytic, tantalum, film, supercap | `CAP`, `CAP-E`, `CAP-T`, `CAP-F`, `CAP-SC` |
| `03_inductors` | Inductors, beads, chokes, transformers | `IND`, `FB`, `CHK`, `XFMR` |
| `04_diodes` | Rectifier, Schottky, Zener, TVS/ESD, bridge, LED | `DIO`, `DIO-S`, `DIO-Z`, `DIO-T`, `DIO-BR`, `LED` |
| `05_transistors` | BJT, MOSFET, JFET, IGBT | `BJT-N`, `BJT-P`, `FET-N`, `FET-P`, `JFET`, `IGBT` |
| `06_ics` | All monolithic silicon, the largest library | `IC-*` (see 2.1) |
| `07_crystals` | Crystals, resonators, oscillators | `XTAL`, `RSN`, `OSC` |
| `08_connectors` | Headers, plugs, sockets, jacks, card slots | `CON-*` (see 2.2) |
| `09_electromechanical` | Switches, relays, buzzers, motors, fans, batteries | `SW`, `SW-DIP`, `RLY`, `BUZ`, `MOT`, `FAN`, `BAT`, `BAT-H` |
| `10_protection` | Fuses, PTC, MOV, gas tubes, EMI filters | `FUSE`, `FUSE-H`, `PPTC`, `MOV`, `GDT`, `FLT` |
| `11_modules` | Pre-built assemblies with their own PCB | `MOD-*` (see 2.3) |
| `12_mechanical` | Board features rather than purchased parts. Electrical function is situational: most entries have none, but a plated mounting hole bonded to GND, a test point or a solder pad does, and still belongs here | `MH`, `TP`, `FID`, `LOGO`, `HS`, `PAD` |

### 2.1 IC subtypes

| Prefix | Family |
|---|---|
| `IC-MCU` | Microcontroller, SoC, FPGA |
| `IC-MEM` | Flash, EEPROM, FRAM, SDRAM |
| `IC-LDO` | Linear regulator, voltage reference |
| `IC-DCDC` | Buck, boost, buck-boost, charge pump |
| `IC-PMIC` | Load switch, battery charger, power sequencer, e-fuse |
| `IC-DRV` | Gate, motor, LED and display drivers |
| `IC-OPA` | Op-amp, comparator, instrumentation amp |
| `IC-ADC` | ADC, DAC, analog switch, mux |
| `IC-LOG` | Gates, buffers, level shifters, shift registers |
| `IC-IF` | USB, CAN, RS-485, Ethernet PHY, digital isolators |
| `IC-RF` | Transceivers, baluns, LNA, SAW filters |
| `IC-SNS` | Temperature, IMU, pressure, current, optical sensors |
| `IC-RTC` | Real-time clocks |
| `IC-OPTO` | Optocouplers, solid-state relays |
| `IC` | Anything that does not fit the above |

### 2.2 Connector subtypes

| Prefix | Family |
|---|---|
| `CON-HDR` | Pin header (male) |
| `CON-SKT` | Socket / receptacle (female) |
| `CON-TB` | Terminal block, screw or spring |
| `CON-FPC` | FFC / FPC |
| `CON-USB` | USB of any generation |
| `CON-RJ` | RJ11 / RJ45 |
| `CON-JST` | Wire-to-board (JST, Molex, JAE...) |
| `CON-CARD` | microSD, SIM, edge |
| `CON-RF` | SMA, U.FL, MMCX |
| `CON-PWR` | Barrel jack, spade, banana |

### 2.3 Module subtypes

`MOD-RF`, `MOD-MCU`, `MOD-GNSS`, `MOD-CAM`, `MOD-DISP`, `MOD-PSU`, `MOD-SNS`.

---

## 3. Naming grammar

```
<PREFIX>_<DESCRIPTOR>-<PACKAGE>
```

* `_` separates the prefix block from the descriptor block. **Exactly one per name.**
* `-` separates fields inside each block.
* The package is always the **last** field.
* Prefixes are uppercase; descriptors keep correct SI casing (`uF`, `MHz`, `mA`).

### 3.1 Parametric vs. MPN

The single most important rule:

> **Commodity parts are named by their parameters. Specific silicon is named by its MPN.**

A 10k 1% 0603 resistor is interchangeable across five manufacturers, so the parameters
are its identity, and the MPN lives in the fields. An STM32G071 is not
interchangeable with anything, the MPN *is* its identity.

| Style | Applies to | Example |
|---|---|---|
| Parametric | R, C, L, FB, diodes, BJT/FET, LED, fuse, MOV | `RES_10K-0.1W-1%-0603` |
| MPN | All `IC-*`, `MOD-*`, `CON-*`, sensors | `IC-MCU_STM32G071CBT6` |
| Hybrid | Crystals, potentiometers, relays, lead with the value | `XTAL_16MHz-ABM8-16.000MHZ-B2-T` |

Never put the manufacturer name in the part name. That is what the `Manufacturer`
field is for.

### 3.2 Descriptor patterns by family

| Family | Pattern | Example |
|---|---|---|
| Resistor | `value-power-tolerance-package` | `RES_4.7K-0.1W-1%-0603` |
| Resistor (precision) | add tempco | `RES_10K-0.1W-0.1%-25ppm-0603` |
| Shunt | `value-power-tolerance-package` | `RES-SH_0.01R-1W-1%-2512` |
| Capacitor (MLCC) | `value-voltage-tolerance-dielectric-package` | `CAP_100nF-50V-10%-X7R-0603` |
| Capacitor (electrolytic) | `value-voltage-tolerance-package` | `CAP-E_100uF-25V-20%-RAD-TH` |
| Inductor | `value-Idc-tolerance-package` | `IND_10uH-1.2A-20%-0805` |
| Ferrite bead | `Z@freq-current-package` | `FB_600R@100MHz-1.5A-0603` |
| Diode | `Vr-If-package` | `DIO_100V-1A-SOD-123` |
| Schottky | `Vr-If-Vf-package` | `DIO-S_40V-1A-0.6V-SOD-123` |
| Zener | `Vz-power-package` | `DIO-Z_5.1V-0.5W-SOD-123` |
| TVS / ESD | `Vrwm-Vc-package` | `DIO-T_5V-9.2V-SOD-523` |
| LED | `color-Vf-If-package` | `LED_RED-2V-20mA-0603` |
| BJT | `Vceo-Ic-package` | `BJT-N_40V-200mA-SOT-23` |
| MOSFET | `Vds-Id-Rds-package` | `FET-N_30V-5A-45mR-SOT-23` |
| Fuse | `current-voltage-package` | `FUSE_3A-32V-1206` |
| MOV | `Vac-Vc-package` | `MOV_275V-710V-DISC-TH` |

Fields that do not apply are omitted, never left blank. Order is fixed, do not
reshuffle to taste.

Zener tolerance is deliberately **not** in the name. The Vz grade is already encoded
in the MPN (`BZT52C5V1` is the 5.1V bin) and it does not change what the part does in
a circuit, so it only makes the name longer. Record it in `Description` when it
matters. Tolerance stays in the name for R, C and L, where it is the whole point of
picking one part over another.

### 3.3 Notation rules

* **Zero ohms** is `0R`. Use `R` as the ohm symbol: `0.01R`, `4.7R`, `1K`, `1M`.
* **Decimal point** for values under one unit: `0.1uF`, not `100n` in a `CAP_` name
  (use `100nF`, pick the unit that avoids a leading zero).
* **Case is exact**: `pF nF uF mF`, `nH uH mH`, `Hz kHz MHz GHz`, `uA mA A`, `mV V kV`,
  `mW W`, `mR R K M`. Never `UF`, `MHZ`, `Ma`.
* **`K` in the name, `k` in the `Value`.** The name uses uppercase `K` for the kilo
  multiplier so every part name reads the same way: `RES_4.7K-0.1W-1%-0603`. The
  `Value` field is what the schematic prints, so it follows SI and uses lowercase
  `k`: `4k7`. `M` is uppercase in both, because SI mega already is.
* **Tolerance** always carries `%`: `1%`, `0.1%`, `20%`.
* **Forbidden characters**: space, comma, `/`, `\`, `:`, `"`, `'`, `#`, `(`, `)`.
  These break KiCad library nicknames, file paths, or BOM CSV export.
* **Never** append `_1`, `_copy`, `_new` to disambiguate. If two parts differ, the
  difference belongs in the descriptor.

### 3.4 Package suffixes

Use the JEDEC or industry name verbatim when one exists.

| Suffix | Meaning |
|---|---|
| `0402` `0603` `0805` `1206` `2512` | Imperial chip sizes |
| `SOT-23` `SOT-23-5` `SOT-223` `SOD-123` | Standard small outline |
| `SOIC-8` `TSSOP-16` `QFN-24` `LQFP-48` `BGA-64` | Standard IC packages |
| `DO-214AC` `DO-214AA` `DO-214AB` | SMA / SMB / SMC |
| `TH` | Generic through-hole, no standard name |
| `RAD-TH` `AXL-TH` | Radial / axial leaded |
| `SMD` | Generic surface mount, no standard name |
| `MOD` | Module with a non-standard outline |

Prefer `TH`/`SMD` only when the part genuinely has no industry package name.

---

## 4. Footprint naming

```
<PACKAGE>[-<VARIANT>][_<MPN>]
```

* Generic footprints use the package name alone: `SOT-23-5`, `QFN-24-4x4-0.5`.
* Manufacturer-specific outlines append the MPN: `LGA-71_NINA-B306`.
* Density variants use the IPC letter: `-L` least, `-N` nominal, `-M` most.
  `SOIC-8-N` is the default when unmarked.
* **Do not create footprints that already exist in the official KiCad libraries.**
  Chip resistors, SOT-23, SOIC and friends come from `Resistor_SMD:`,
  `Package_TO_SOT_SMD:`, `Package_SO:`. This library holds only proprietary or
  corrected outlines.

## 5. 3D model naming

The 3D file is named **exactly like the footprint it belongs to**, with the model
extension. `LGA-71_NINA-B306.kicad_mod` pairs with `LGA-71_NINA-B306.step`.

Reference it through the library path variable, never a relative path:

```
${KICAD_MYLIB}/3d/LGA-71_NINA-B306.step
```

---

## 6. Reference designators

Set on the symbol, per IEEE 315 / ASME Y14.44.

| Des | Part | Des | Part |
|---|---|---|---|
| `R` | Resistor | `D` | Diode, LED |
| `RN` | Resistor array | `Q` | Transistor |
| `RV` | Potentiometer | `U` | Integrated circuit, module |
| `RT` | Thermistor | `Y` | Crystal, oscillator |
| `C` | Capacitor | `J` | Connector (fixed) |
| `L` | Inductor | `P` | Connector (mating) |
| `FB` | Ferrite bead | `S` | Switch |
| `T` | Transformer | `K` | Relay |
| `F` | Fuse | `LS` | Buzzer, speaker |
| `MOV` | Varistor | `M` | Motor, fan |
| `BT` | Battery | `H` | Mounting hardware |
| `TP` | Test point | `FID` | Fiducial |

---

## 7. Required fields

Every symbol carries these. A field is left empty only when it genuinely does not
apply to the part, never because the value was not looked up. See 7.1.

**Always:**
`Reference` · `Value` · `Footprint` · `Datasheet` · `Description` ·
`MPN` · `Manufacturer` · `Package`

**Distributors**: at least one, as its own field so a second source is one lookup away:
`Digikey` · `Mouser` · `LCSC`

**Per family:**

| Family | Additional |
|---|---|
| Resistor | `Tolerance`, `Power`, `TempCoeff` (precision only) |
| Capacitor | `Tolerance`, `Voltage`, `Dielectric` |
| Inductor | `Tolerance`, `Idc` |

**Recommended:** `Height`, `MPN2` / `Manufacturer2`, `Temp_Range`, `RoHS`, `AEC-Q`.

A `0R` jumper carries no `Tolerance`: the part is specified by a maximum resistance,
not by a percentage, so the field does not apply and is omitted per 3.2.

`Description` must be human-readable and searchable, it is what the symbol chooser
filters on. Write `Resistor 10k 1% 0.1W 0603 thick film`, not `res`.

**Never store price, stock or lifecycle status in a field.** Those go stale within
weeks and turn the library into a source of wrong information. A stale `Active` is
worse than no value at all, because it looks authoritative. Pricing and lifecycle
belong to the BOM tooling, resolved live from the MPN at design time.

### 7.1 When a field may be left empty

The list above assumes a purchased part: something with a manufacturer, an order
code, and a line on the BOM. Not every symbol is one. A mounting hole, a fiducial,
a test point, a logo, a set of pads that exists only to receive a soldered wire,
none of these are bought, so `MPN`, `Manufacturer` and the distributor fields have
nothing to hold.

Omit a field when it **does not apply**, and only then:

| Case | Fields that do not apply |
|---|---|
| Board feature, not a part: `MH`, `TP`, `FID`, `LOGO`, `PAD` | `MPN`, `Manufacturer`, distributors |
| Land pattern used bare, e.g. wires soldered into a header footprint | `MPN`, `Manufacturer`, distributors, until it is populated |
| Part where a value is genuinely not specified, per 3.2 | that field alone, e.g. `Tolerance` on a `0R` |

Anything in the first two rows is marked `in_bom no`. That is what makes the
omission correct rather than sloppy: the symbol never reaches a BOM, so there is
nothing to order and no field to fill. The moment a symbol is `in_bom yes`,
section 7 applies to it in full, with no exceptions.

The distinction that matters is **omitted because inapplicable, not blank because
nobody looked it up.** A missing `MPN` on a mounting hole is correct. A missing
`MPN` on a regulator is an unfinished part, and it is worse than an obviously
wrong one, because nothing about it looks unfinished at the point of use.

---

## 8. Worked examples

```
RES_10K-0.1W-1%-0603
RES_0R-0.125W-1%-0805
RES-A_4.7K-0.2W-2%-SIP-7
RES-SH_0.005R-2W-1%-2512
NTC_10K-3435-0603
POT_10K-3296W-1-103RLF-TH

CAP_100nF-50V-10%-X7R-0603
CAP_22pF-50V-5%-C0G-0402
CAP-E_100uF-25V-20%-RAD-TH
CAP-T_10uF-16V-10%-DO-214AA

IND_10uH-1.2A-20%-0805
FB_600R@100MHz-1.5A-0603

DIO-S_40V-1A-0.6V-SOD-123
DIO-Z_5.1V-0.5W-SOD-123
DIO-T_5V-9.2V-SOD-523
LED_GREEN-2.1V-20mA-0603

BJT-N_40V-200mA-SOT-23
FET-N_30V-5A-45mR-SOT-23

IC-MCU_STM32G071CBT6
IC-MEM_W25Q64JVSSIQ
IC-LDO_AP2112K-3.3TRG1
IC-DCDC_MP2322GQH-P
IC-SNS_HS3003
IC-IF_USBLC6-2SC6

XTAL_16MHz-ABM8-16.000MHZ-B2-T
CON-USB_ZX62-AB-5PA31
CON-HDR_2x5-1.27-SMD
MOD-RF_NINA-B306
FUSE_3A-32V-1206
MH_M3-3.2D-6.4P
TP_1.0D-SMD
```
