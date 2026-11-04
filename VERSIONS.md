# MSG gripper versions

Which MSG gripper you have, and which files, firmware and manual go with it.

## Releases

| Release | Date | What it is | Firmware | Files |
|---|---|---|---|---|
| **V1.2** | 2026-11-04 | the files as of November 2026: updated body parts, sliders, ring cover and jaws, the body for the 60 mm stepper, more printable parts and a guide to the jaws, a reference image for every BOM part, the URDF and MuJoCo models in `msg_description` | STEPFOC 108 | [download](https://github.com/Source-Robotics/MSG-compliant-AI-stepper-gripper/releases/tag/V1.2) |
| V1.1 | 2026-02-19 | the first beta release: the BOM, the STEP files for the 100, 150 and 200 mm rails and the 21.5, 40 and 60 mm steppers, the assembly video | STEPFOC 103 to 108 | [tag](https://github.com/Source-Robotics/MSG-compliant-AI-stepper-gripper/releases/tag/V1.1) |

## Which version do I have?

The gripper comes in nine variants: any of three rails with any of three steppers, in a black or grey body.

| | What differs | How to tell |
|---|---|---|
| Rail: 100, 150 or 200 mm | the stroke: 55, 105 or 155 mm; the body and the jaw carriers are printed for the rail | the length of the rail; the shop names it by its stroke |
| Stepper: 21.5, 40 or 60 mm | the grip force: about 31, 50 or 76 N at the stepper's maximum current (1.5, 2.0 or 2.1 A); the stepper cover is printed for the stepper | the length of the stepper; the motors are the 17HS08-1004S, 17HS16-2004S1 and 17HS24-2104S |
| Release V1.1 or V1.2 | V1.2 updates the body parts, sliders, ring cover and jaws | both run STEPFOC firmware 108, which the current manual describes |

## What works together

The STEPFOC firmware, the bootloader, the Python library, the ROS 2 driver and the manual for each release:
[Versions and compatibility](https://docs.source-robotics.com/MSG-gripper-docs/latest/appendix/compatibility/) in the manual.
