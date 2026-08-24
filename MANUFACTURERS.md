# Preferred Manufacturers

A sourcing reference for this library. For each category it lists the three
manufacturers to prefer when reliability matters, plus the practical choice for
cost-driven or JLCPCB-assembled builds.

These are guidelines, not rules. The right answer is always the part whose
datasheet covers your operating conditions with margin, this table only says
where to look first.

## How to read this

**Quality tier**: reach for these when the part sits in a signal path that
matters, carries real power, faces the outside world, or must survive
temperature extremes. They publish complete characterization data, hold tighter
process control, and stay in production longer.

**Volume tier**: adequate for the majority of positions in a typical board,
pull-ups, decoupling, indicator LEDs, generic switching. Usually *Basic* parts
on JLCPCB, meaning no per-part setup fee.

Mixing tiers on one board is normal and expected. A design that uses Yageo for
forty pull-ups and Panasonic for two feedback dividers is making the right call,
not cutting corners.

---

## 01 - Resistors

| Tier | Manufacturers |
|---|---|
| Quality | Panasonic (ERJ, ERA) · KOA Speer (RK73) · Vishay (CRCW, TNPW) |
| Volume | Yageo (RC, AC) · Uniroyal (recommended only for non-critical positions) |

Panasonic ERA and Vishay TNPW are thin film, so reach for them when tempco or
long-term drift matters: feedback dividers, current-sense scaling, ADC
references. Thick film (ERJ, RC) is fine everywhere else. Susumu RG is the
precision benchmark but is rarely stocked outside the major distributors.

## 02 - Capacitors

| Type | Quality | Volume |
|---|---|---|
| MLCC | Murata (GRM, GCM) · TDK (C, CGA) · Samsung Electro-Mechanics (CL) | Samsung CL · Walsin |
| Aluminum electrolytic | Nichicon · Panasonic · Rubycon | Lelon · Aishi |
| Tantalum / polymer | KEMET · Kyocera AVX · Vishay | - |

Murata and TDK publish the most complete DC bias and temperature derating data,
which matters more than the tolerance printed on the part: a Class II MLCC can
lose most of its rated capacitance under working voltage. Use them wherever the
actual capacitance is load-bearing, DC-DC output filters, ADC references, PLL
loop filters. For aluminum electrolytics, ripple current rating and rated
lifetime at temperature are the specs that separate the tiers.

## 03 - Inductors

| Type | Quality | Volume |
|---|---|---|
| Power inductors | Coilcraft (XAL, XEL) · Murata (DFE) · TDK (TFM) | Sunlord · Chilisin |
| Ferrite beads | Murata (BLM) · TDK (MPZ) · Laird | Sunlord |
| Common-mode chokes | Würth (WE-CNSW) · TDK · Murata | - |

Coilcraft leads on saturation behavior and publishes soft-saturation curves
instead of a single Isat number, which is what you actually need when sizing a
buck converter. Würth is worth knowing for its free sample program and
application support.

## 04 - Diodes

| Type | Quality | Volume |
|---|---|---|
| Rectifier / Schottky | Nexperia · onsemi · Vishay | Diodes Inc · Changjiang (CJ) |
| Zener | Nexperia · onsemi · Vishay | Changjiang (BZT52 series) |
| TVS / ESD | Littelfuse · Semtech · Nexperia (PESD) | ProTek · Changjiang |
| LEDs | Nichia · Cree · Osram | Everlight · Hubei KENTO (KT series) |

ESD protection is the one place not to economize. Clamping voltage and dynamic
resistance vary widely between a characterized part and a generic one, and the
difference only shows up as field failures. Littelfuse and Semtech publish
transmission-line-pulse data; most low-cost vendors do not.

## 05 - Transistors

| Type | Quality | Volume |
|---|---|---|
| Small-signal BJT | Nexperia · onsemi · ROHM | Changjiang · Slkor |
| MOSFET (logic level) | Infineon · onsemi · Toshiba | Alpha & Omega (AO3400A, AO3401A) · Diodes Inc |
| MOSFET (power) | Infineon (OptiMOS) · onsemi · Toshiba | Alpha & Omega |

Alpha & Omega parts are genuinely good value and dominate the JLCPCB Basic list
for switching duty. Move to Infineon or Toshiba when thermals get tight, when
switching losses matter at high frequency, or when the part is in a safety path.

## 06 - Integrated circuits

| Type | Preferred |
|---|---|
| Regulators, DC-DC | Texas Instruments · Analog Devices · Monolithic Power Systems |
| MCU, SoC | STMicroelectronics · Nordic Semiconductor · Espressif · Microchip |
| Op-amps, ADC/DAC | Analog Devices · Texas Instruments · ROHM |
| Logic, level shift | Nexperia · Texas Instruments · onsemi |
| Interface, isolation | Texas Instruments · Analog Devices · Silicon Labs |
| Memory | Winbond · Macronix · Micron |
| Sensors | Bosch Sensortec · Sensirion · TDK InvenSense |

For ICs the choice is driven by the function, not by a vendor ranking. What is
worth standardizing on is the *ecosystem*: pick a small number of vendors whose
evaluation tools, reference designs, and support you know well. A slightly worse
part with a simulation model and a proven layout beats a better part you have to
characterize yourself.

## 07 - Crystals and oscillators

| Tier | Manufacturers |
|---|---|
| Quality | Epson · NDK · Murata (resonators) |
| Volume | Abracon · ECS · Yangxing (YXC) |

Match the load capacitance to the oscillator circuit, and check the drive level
rating. Most crystal failures in the field trace back to one of those two
numbers, not to the manufacturer.

## 08 - Connectors

| Type | Quality | Volume |
|---|---|---|
| Wire-to-board | JST · Molex · TE Connectivity | HRO · XKB |
| Board-to-board | Hirose · Molex · Samtec | - |
| Headers | Würth · Harwin · Amphenol | - |
| USB, RF | Hirose · Amphenol · TE Connectivity | Korean Hroparts |

Connectors are mechanical parts and fail mechanically. Mating cycle rating and
retention force are the specs that matter, and they are where the low-cost
options genuinely differ. Anything a user will plug and unplug deserves a
tier-one part.

## 09 - Electromechanical

| Type | Quality | Volume |
|---|---|---|
| Tactile switches | C&K · Omron · Alps Alpine | XKB · Kailh |
| Slide, DIP, rotary | C&K · Nidec Copal · Alps Alpine | - |
| Relays | Omron · Panasonic · TE Connectivity | HF (Hongfa) |
| Buzzers | Murata · TDK · PUI Audio | - |
| Batteries, holders | Panasonic · Keystone · MPD | - |

Hongfa relays are widely used in production and are a reasonable exception to
the tier split, they are a mainstream choice, not a compromise.

## 10 - Protection

| Type | Preferred |
|---|---|
| Fuses, PPTC | Littelfuse · Bourns · Eaton (Bussmann) |
| MOV, GDT | Littelfuse · Bourns · TDK (EPCOS) |
| EMI filters | Murata · TDK · Würth |

No volume tier here on purpose. Protection components only earn their place if
they behave predictably at the moment they are needed, and that is exactly the
condition an uncharacterized part cannot be trusted at. Buy these from vendors
with real qualification data and traceable lots.

## 11 - Modules

| Type | Preferred |
|---|---|
| RF, BLE, Wi-Fi | u-blox · Nordic Semiconductor · Espressif |
| GNSS | u-blox · Quectel · SkyTraq |
| Cellular | Quectel · Telit · Sierra Wireless |

Check certification coverage before anything else, FCC, CE, ANATEL and the
rest. A pre-certified module is often the entire reason to use a module instead
of a chip-down design, and that value disappears if the certificate does not
cover your target market.

## 12 - Mechanical

| Type | Preferred |
|---|---|
| Standoffs, spacers | Würth · Keystone · Essentra |
| Heatsinks | Wakefield-Vette · Aavid · Fischer Elektronik |
| Test points, fiducials | Keystone · Harwin |

---

## Lifecycle

Whatever the manufacturer, check lifecycle status before committing a part to
the library. A part marked NRND today is a redesign in eighteen months.
Manufacturer product pages and distributor lifecycle indicators are the primary
sources; treat any part you cannot find a status for as a risk.

The status is deliberately **not** stored in a field. It goes stale exactly the way
price and stock do, and a stale `Active` sitting in the library is worse than no
value at all. Check it against the distributor at design time, every time.

For anything expected to stay in production, fill in `MPN2` and `Manufacturer2`
with a qualified second source at the time of design, not at the time of
shortage.
