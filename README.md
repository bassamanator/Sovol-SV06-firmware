# 🚨 _One-Stop-Shop_ Klipper Configuration

This branch contains the OSS Klipper configuration that can be used with **any printer** running Klipper.

| Printer                                                                 | Branch                                                                                    |
| ----------------------------------------------------------------------- | ----------------------------------------------------------------------------------------- |
| Sovol SV06                                                              | [master](https://github.com/bassamanator/Sovol-SV06-firmware/tree/master)                 |
| Sovol SV06 Skr-Mini-E3-V3.0                                             | [skr-mini-e3-v3](https://github.com/bassamanator/Sovol-SV06-firmware/tree/skr-mini-e3-v3) |
| Sovol SV06 Plus                                                         | [sv06-plus](https://github.com/bassamanator/Sovol-SV06-firmware/tree/sv06-plus)           |
| ${\normalsize{\textcolor{darkturquoise}{\texttt{All other printers}}}}$ | ⚡ $\small{\textcolor{darkturquoise}{\text{YOU ARE HERE}}}$ ⚡                            |

I am creating these files for my personal use and cannot be held responsible for what it might do to your printer. Use at your own risk.

## Outline

- [Features](#features)
- [Stay Up-to-Date](#stay-up-to-date)
- [Before You Begin](#before-you-begin)
- [Installation Steps](#installation-steps)
  - [Download OSS Klipper Configuration](#download-oss-klipper-configuration)
- [Adjust Your Slicer](#adjust-your-slicer)
- [Directory Structure](#directory-structure)
- [Support Me](#support-me)
- [FAQ](#faq)
- [Useful Resources](#useful-resources)
- [Sovol Official Links](#sovol-official-links)
- [Sources](#sources)

## Features

- 💥 This Klipper configuration is an _endpoint_, meaning that it contains **everything** that you could possibly need in order to have an excellent Klipper experience! 💥 CoreXY users can rightly disagree and say that it lacks the quad gantry levelling macros. Please create a pull request if you can help in this regard!
- Filament runout sensor usage implemented.
- Minimum configuration settings for Mainsail/Fluiddpi to work.
- A SuperSlicer config bundle that contains what are considered by many to be the best print settings available for any FDM printer ([Ellis' SuperSlicer Profiles](https://github.com/AndrewEllis93/Ellis-SuperSlicer-Profiles)).
- `NEW` <img src="./images/party_blob.gif" width="20" alt=''/> A PrusaSlicer config bundle based on Ellis' SuperSlicer Profiles.
- Macros
  - **Improved** mechanical gantry calibration/`G34` macro that provides the user audio feedback, and time to check the calibration. ⚠️ This is for i3 style printers only, see example video [here](https://youtu.be/aVdIeIIpUAk).
  - Misc macros: `PRINT_START`, `CANCEL_PRINT`, `PRINT_END`, `PAUSE`, `RESUME`.
  - Parking macros (parks the printhead at various locations): `PARKFRONT`, `PARKFRONTLOW`, `PARKREAR`, `PARKCENTER`, `PARKBED`.
  - Load/unload filament macros.
  - Purge line macro.
  - `NEW` <img src="./images/party_blob.gif" width="20" alt=''/> `TEST_SPEED` macro. Find instructions [here](https://github.com/bassamanator/Sovol-SV06-firmware/blob/master/README.md#how-do-i-use-the-test_speed-macro).
- `NEW` <img src="./images/party_blob.gif" width="20" alt=''/> Klipper Adaptive Meshing & Purging (KAMP) added (disabled by default)! Read about it [here](https://github.com/bassamanator/Sovol-SV06-firmware/blob/master/README.md#how-do-i-enable-kamp-klipper-adaptive-meshing--purging).

## Stay Up-to-Date

⭐ ${\normalsize{\textcolor{goldenrod}{\texttt{Star this project}}}}$

Watch for releases and updates.

<img src="./images/githubstar.gif" width="500" alt='githubstar'/>

## Before You Begin

- It is assumed that you already have a working `printer.cfg` and you already have your printer up and running Klipper.
- Know what you're getting into by reading this documentation _fully!_
- It is assumed that you are connected to your host Raspberry Pi (or other host device) via SSH, and that your printer motherboard is connected to the host via a data USB cable.
- It is assumed that the username on the host device is `pi`. If that is not the case, you will have to manually edit `moonraker.conf` and `cfgs/misc-macros.cfg` and change any mentions of `/home/pi` to `/home/yourUserName`.
- I would recommend searching for the word `NOTE` in this repository. There are roughly half a dozen short points amongst the various files that you should be aware of if you're using this configuration.

## Installation Steps

### Download OSS Klipper Configuration

1. [Download](https://github.com/bassamanator/Sovol-SV06-firmware/archive/refs/heads/any-printer.zip) the `ZIP` file containing the Klipper configuration.
2. The parent folder in the `ZIP` is `Sovol-SV06-firmware-any-printer`. This is relevant in the next step.
3. Extract **only** the _contents_ of the parent folder into `~/printer_data/config`.

💡 **If** you get a warning that you already have a `moonraker.conf` (which you probably do since you're already up and running Klipper), **and** you're not using a low powered device such as a Raspberry Pi Zero, you need to simply paste the following into your <u>_existing_</u> `moonraker.conf`:

```
[file_manager]
enable_object_processing: True
```

### Setup Instructions

Simply add `[include ./osskc.cfg]` somewhere at the top of your `printer.cfg`.

## Adjust Your Slicer

You need to adjust the start and end gcode in your slicer. The relevant macros are `PRINT_START` and `PRINT_END`. Find instructions [here](https://ellis3dp.com/Print-Tuning-Guide/articles/passing_slicer_variables.html#slicer-start-g-code).

If you would like to print a purge line before your print starts, at the end of your start gcode, on a new line add `PURGE_LINE`. ⚠️ This is just an **example**:

```
PRINT_START ...
PURGE_LINE
```

## Directory Structure

This repository contains many files and folders. Some are _necessary_ for this Klipper configuration to work, others are not.

- **Necessary** items are marked with a ✅.
- Items that can _optionally_ be deleted are marked with a ❌.

```
├── cfgs ✅
│   ├── adxl-direct.cfg
│   ├── adxl-rp2040.cfg
│   ├── adxl-rpi-pico-2x.cfg
│   ├── MECHANICAL_GANTRY_CALIBRATION.cfg
│   ├── misc-macros.cfg
│   ├── PARKING.cfg
│   └── TEST_SPEED.cfg
├── CODE_OF_CONDUCT.md ❌
├── CONTRIBUTING.md ❌
├── .github ❌
│   └── ISSUE_TEMPLATE
│       ├── bug_report.md
│       └── feature_request.md
├── .gitignore ❌
├── images ❌
│   ├── cup-border.png
│   ├── githubstar.gif
│   ├── heart.gif
│   ├── logo_white_stroke.png
│   └── party_blob.gif
├── misc ❌
│   └── SuperSlicer_config_bundle.ini
├── moonraker.conf ✅ ❌ ¿? (depends if you already have this file or not)
├── osskc.cfg ✅
├── README.md ❌
└── .vscode ❌
    └── settings.json
```

## Support Me

${\normalsize{\textcolor{goldenrod}{\texttt{Please ⭐ star this repository!}}}}$

If you found my work useful, consider buying me a [<img src="./images/logo_white_stroke.png" height="20" alt='Ko-fi'/>](https://ko-fi.com/bassamanator).

## FAQ

Please find answers to common questions [here](https://github.com/bassamanator/Sovol-SV06-firmware/blob/master/README.md#faq).

## Useful Resources

- [Everything Sovol SV06](https://github.com/bassamanator/everything-sovol-sv06)
- [RP2040-Zero ADXL345 Connection Klipper](https://github.com/bassamanator/rp2040-zero-adxl345-klipper)
- ⭐⭐⭐⭐⭐ [Ellis' Print Tuning Guide](https://ellis3dp.com/Print-Tuning-Guide)
- [Simplify3D Print Quality Troubleshooting Guide](https://www.simplify3d.com/resources/print-quality-troubleshooting/)

## Sovol Official Links

- [SV06 Marlin Source Code](https://github.com/Sovol3d/Sv06-Source-Code)
- [SV06 Models](https://github.com/Sovol3d/SV06-Fully-Open-Source)
- [SV06 Plus Marlin Source Code and Models](https://github.com/Sovol3d/SV06-PLUS)

## Sources

- [https://www.klipper3d.org](https://www.klipper3d.org)
- [Ellis' Print Tuning Guide](https://ellis3dp.com/Print-Tuning-Guide)
- [Mechanical Gantry Calibration Macro](https://github.com/strayr/strayr-k-macros)
- [SV06 printer.cfg](https://github.com/spinixguy/Sovol-SV06-firmware)
- [SV06 Buildplate and Texture](https://www.printables.com/model/378915-sovol-sv06-buildplate-texture-and-model-for-prusas)
- [Ellis' SuperSlicer Profiles](https://github.com/AndrewEllis93/Ellis-SuperSlicer-Profiles)
- [Klipper Adaptive Meshing & Purging](https://github.com/kyleisah/Klipper-Adaptive-Meshing-Purging)
- [PrusaSlicer Print Settings](https://github.com/mjonuschat/PrusaSlicer-Profiles)

[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/H2H0HIHTH)
