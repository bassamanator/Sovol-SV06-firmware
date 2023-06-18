# 🚨 _One-Stop-Shop_ Klipper Configuration

This branch contains the OSS Klipper configuration that can be used with **any printer** running Klipper.

For the **Sovol SV06**, please refer to the [master](https://github.com/bassamanator/Sovol-SV06-firmware/tree/master) branch.

For the **Sovol SV06 SKR-Mini-E3-V3**, please refer to the [skr-mini-e3-v3](https://github.com/bassamanator/Sovol-SV06-firmware/tree/skr-mini-e3-v3) branch.

For the **Sovol SV06 Plus**, please refer to the [sv06-plus](https://github.com/bassamanator/Sovol-SV06-firmware/tree/sv06-plus) branch.

# Highlights

- 💥 This Klipper configuration is an _endpoint_, meaning that it contains **everything** that you could possibly need in order to have an excellent Klipper experience! 💥 CoreXY users can rightly disagree and say that it lacks the quad gantry leveling macros. Please create a pull request if you can help in this regard!
- `NEW` <img src="./images/party_blob.gif" width="20" alt=''/> Filament runout sensor usage implemented. <img src="./images/party_blob.gif" width="20" alt=''/>
- Minimum configuration settings for Mainsail/Fluiddpi to work.
- SuperSlicer config bundle that contains the printer configuration, as well as what are considered by many to be the best print settings available for any FDM printer ([Ellis' SuperSlicer Profiles](https://github.com/AndrewEllis93/Ellis-SuperSlicer-Profiles)). Find the differences between the different print setting profiles [here](https://github.com/AndrewEllis93/Ellis-SuperSlicer-Profiles/tree/master/SuperSlicer). But basically, the 45 degree profile places the seam at the back.
- Macros
  - **Improved** mechanical gantry calibration/`G34` macro that provides the user audio feedback, and time to check the calibration. ⚠️ This is for i3 style printers only, see example video [here](https://youtu.be/aVdIeIIpUAk).
  - Misc macros: `PRINT_START`, `CANCEL_PRINT`, `PRINT_END`, `PAUSE`, `RESUME`.
  - Parking macros (parks the printhead at various locations): `PARKFRONT`, `PARKFRONTLOW`, `PARKREAR`, `PARKCENTER`, `PARKBED`.
  - Load/unload filament macros.
  - Purge line macro.

## Stay Up-to-Date

I work on this repository all the time and a lot of new features are coming. Watch releases of this repository to be notified for future updates:

<img src="./images/githubstar.gif" width="500" alt='Raspberry Pi'/>

# Installation Steps

## Before You Begin

- Know what you're getting into by reading this documentation _fully!_
- It is assumed that you are connected to your host Raspberry Pi (or other host device) via SSH, and that your printer motherboard is connected to the host via a data USB cable.
- It is assumed that the username on the host device is `pi`. If that is not the case, you will have to manually edit `moonraker.conf` and `cfgs/misc-macros.cfg` and change any mentions of `/home/pi` to `/home/yourUserName`.
- It is assumed that you already have a working `printer.cfg` and you already have your printer up and running Klipper.
- I would recommend searching for the word `NOTE` in this repository. There are roughly half a dozen short points amongst the various files that you should be aware of if you're using this configuration.

## Download the Configuration

1. [Download](https://github.com/bassamanator/Sovol-SV06-firmware/archive/refs/heads/any-printer.zip) the `ZIP` file containing the Klipper configuration.
2. The parent folder in the `ZIP` is `Sovol-SV06-firmware-any-printer`. This is relevant in the next step.
3. Extract **only** the _contents_ of the parent folder into `~/printer_data/config`.

💡 If you get a warning that you already have a `moonraker.conf` (which you probably do since you're already up and running Klipper), and _you're not using a low powered device such as the RPi Zero_, you need to simply paste the following into your _existing_ `moonraker.conf`:

```
[file_manager]
enable_object_processing: True
```

## Setup Instructions

Simply add `[include ./osskc.cfg]` somewhere at the top of your `printer.cfg`.

## Directory Structure

This repository contains many files and folders. Some are _necessary_ for this Klipper configuration to work, others are not.

- **Necessary** items are marked with a ✅.
- Items that can _optionally_ be deleted are marked with a ❌.

```
├── cfgs ✅
│   ├── adxl-direct.cfg
│   ├── adxl-rp2040.cfg
│   ├── MECHANICAL_GANTRY_CALIBRATION.cfg
│   ├── misc-macros.cfg
│   ├── PARKING.cfg
│   └── TEST_SPEED.cfg
├── .vscode❌
├── .gitignore❌
├── images ❌
│   ├── githubstar.gif
│   ├── heart.gif
│   └── party_blob.gif
├── misc ❌
│   ├── cup-border.png
│   ├── logo_white_stroke.png
│   └── SuperSlicer_config_bundle.ini
├── moonraker.conf ✅ ❌ ¿? (depends if you already have this file or not)
├── osskc.cfg ✅
└── README.md ❌
```

## <img src="./images/cup-border.png" width="30" alt='Ko-fi'/> Support Me <img src="./images/cup-border.png" width="30" alt='Ko-fi'/>

<img src="./images/heart.gif" width="17" alt=''/> If you found my work useful, please consider buying me a [<img src="./images/logo_white_stroke.png" height="20" alt='Ko-fi'/>](https://ko-fi.com/bassamanator).

## FAQ

Please find answers to common questions [here](https://github.com/bassamanator/Sovol-SV06-firmware/blob/master/README.md#faq).

## Useful Resources

- [Everything Sovol SV06](https://github.com/bassamanator/everything-sovol-sv06)
- [RP2040-Zero ADXL345 Connection Klipper](https://github.com/bassamanator/rp2040-zero-adxl345-klipper)
- ⭐⭐⭐⭐⭐ [Ellis' Print Tuning Guide](https://ellis3dp.com/Print-Tuning-Guide)
- [Simplify3D Print Quality Troubleshooting Guide](https://www.simplify3d.com/resources/print-quality-troubleshooting/)

## Links

- [SV06 Official Marlin Source Code](https://github.com/Sovol3d/Sv06-Source-Code)
- [SV06 Official Models](https://github.com/Sovol3d/SV06-Fully-Open-Source)
- [SV06 Plus Official Marlin Source Code and Models](https://github.com/Sovol3d/SV06-PLUS)

## Sources

- https://www.klipper3d.org
- https://ellis3dp.com/Print-Tuning-Guide
- https://github.com/strayr/strayr-k-macros
- https://docs.vorondesign.com/build/software/miniE3_v20_klipper.html
- https://github.com/spinixguy/Sovol-SV06-firmware
- https://www.printables.com/model/378915-sovol-sv06-buildplate-texture-and-model-for-prusas
- https://github.com/AndrewEllis93/Ellis-SuperSlicer-Profiles

[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/H2H0HIHTH)