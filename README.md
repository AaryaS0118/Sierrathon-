# Sierrathon PCB Design – Pocket Lab

KiCad 10 project for the Sierrathon PCB design hackathon.
Pocket Lab: a keyring-sized USB logic analyzer / voltmeter for engineering students.
Specs, BOM and scoring live in `Resources/`. Team hub: https://claude.ai/artifact/RmexhoVS2DCETLfirbf43f

## Board outline (done)
- Rounded stick, 75 x 27 mm. USB-C (J1) on the left end; small 3 mm corners there, 6 mm corners on the far end.
- M4 holes: MH1/MH3 at (13, 6)/(13, 21) right behind the USB-C (they take plug-in force),
  MH2/MH4 at (69, 6)/(69, 21), concentric with the far corners. MH4 doubles as the keyring hole.
  Coordinates are mm from the board's top-left (grid origin is set there).
- Blue drawings on `User.Comments` are placement guides only (J1, J2 header row, RP2040 zone,
  LED zone, hole circles). Delete them once parts are placed.
- J2 (1x12 header, bottom edge) is drawn as a guide only. It goes in the schematic only after
  written judge approval.

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
