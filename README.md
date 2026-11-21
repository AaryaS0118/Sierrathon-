# Sierrathon PCB Design – Pocket Lab

KiCad 10 project for the Sierrathon PCB design hackathon.
Pocket Lab: a keyring-sized USB logic analyzer / voltmeter for engineering students.
Specs, BOM and scoring live in `Resources/`. Team hub: https://claude.ai/artifact/RmexhoVS2DCETLfirbf43f

## Board outline (done)
- Rounded stick, 75 x 27 mm, 3 mm corners. USB-C on BOTH ends:
  J1 (left, silkscreen "PC") goes to the laptop, J2 (right, silkscreen "PROBE") goes to the circuit under test.
- M4 holes sit right behind each USB-C so the screws take plug-in force:
  MH1/MH3 at (13, 6)/(13, 21), MH2/MH4 at (62, 6)/(62, 21). MH4 doubles as the keyring hole.
  Coordinates are mm from the board's top-left (grid origin is set there).
- Blue drawings on `User.Comments` are placement guides only (J1, J2, RP2040 zone, LED zone,
  R20-R25 zone, hole circles). Delete them once parts are placed.

## Probe port J2 (judges approved adding a USB-C)
J2 is the SAME part as J1 (UJ20-C-H-G-SMT-TR), so no new footprint. Each signal goes through
a 1 kΩ series resistor (R20-R25, same part as R7/R8: RC0402FR-071KL) placed close to J2.

| J2 pins | Net | RP2040 |
|---|---|---|
| A6 + B6 (D+, tied together) | CH0 | GPIO0 (pin 2) |
| A7 + B7 (D-, tied together) | CH1 | GPIO1 (pin 3) |
| A8 (SBU1) | CH2 | GPIO2 (pin 4) |
| B8 (SBU2) | CH3 | GPIO3 (pin 5) |
| A5 (CC1) | A0 | GPIO26 (pin 38) |
| B5 (CC2) | A1 | GPIO27 (pin 39) |
| GND pins + shell | GND | |
| VBUS pins | not connected (so a charger/laptop plugged in here can't push 5 V in) | |

Flipping the plug swaps CH2/CH3 and A0/A1 only. Draw J2 + R20-R25 on a second KiCad 10
top-level sheet and connect with **global** labels (CH0-CH3, A0, A1). On page 1 only GPIO0-3
and GPIO26-27 lose their no-connect X and get those labels.
BOM changes: USB-C line becomes J1, J2 qty 2; 1 kΩ line becomes R7, R8, R20-R25 qty 8.

## Team rules for working in this repo
- KiCad files don't merge well. **Only one person edits a given file at a time**
  (`.kicad_sch` = schematic, `.kicad_pcb` = board). Say so in the team chat before you start.
- Before you open KiCad: `git pull`. When you're done: save, close KiCad, then commit and push.
- Never commit while the file is open and unsaved.

## Design rules (already set up – don't change without telling the team)
- 4 layers, 62 mil, 1 oz copper, FR-370HR, green mask / white silk (Board Setup > Physical Stackup).
- Constraints page floors are 4 mil clearance / 5 mil track **on purpose**: they must equal the
  90 Ω USB pair geometry (5 mil trace, 4 mil gap). Everything else is held to 6 mil by net
  classes and by `Sierrathon-PCB Design.kicad_dru` (Board Setup > Custom Rules).
- Net classes: `USB_90R` (USB_P/N, USBR_P/N, D_P/D_N – use the diff-pair router, key `6`),
  `Power` (VBUS, +3.3V, +1.1V, GND – 15 mil), `Default` (8 mil).
- Net names in the schematic must match the PDF exactly or nets won't land in the right class.
