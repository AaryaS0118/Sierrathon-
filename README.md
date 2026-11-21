# Sierrathon PCB Design – SeeBraille

KiCad 10 project for the Sierrathon PCB design hackathon.
Specs, BOM, scoring and project idea live in `Resources/`.

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
