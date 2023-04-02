# 🚨 *One-Stop-Shop* Klipper Configuration

This branch contains the OSS Klipper configuration that can be used with **any printer** running Klipper.

If you were looking for the OSS Klipper configuration for the **Sovol SV06**, please switch to the [master](https://github.com/bassamanator/Sovol-SV06-firmware) branch.

# Highlights

- 💥 This Klipper configuration is an *endpoint*, meaning that it contains **everything** that you could possibly need in order to have an excellent Klipper experience! 💥 Voron users can rightly disagree and say that it lacks the quad gantry leveling macros. Please create a pull request if you can help in this regard!
- `NEW` <img src="./images/party_blob.gif" width="20" alt=''/> Filament runout sensor usage implemented. <img src="./images/party_blob.gif" width="20" alt=''/> 
- Minimum configuration settings for Mainsail/Fluiddpi to work.
- SuperSlicer config bundle that contains the printer configuration, as well as what are considered by many to be the best print settings available for any FDM printer ([Ellis' SuperSlicer Profiles](https://github.com/AndrewEllis93/Ellis-SuperSlicer-Profiles)). Find the differences between the different print setting profiles [here](https://github.com/AndrewEllis93/Ellis-SuperSlicer-Profiles/tree/master/SuperSlicer). But basically, the 45 degree profile places the seam at the back.
- Macros
  - **Improved** mechanical gantry calibration/`G34` macro that provides the user audio feedback, and time to check the calibration. ⚠️ This is for i3 style printers only, see example video [here](https://youtu.be/aVdIeIIpUAk).
  - Misc macros: `PRINT_START`, `CANCEL_PRINT`, `PRINT_END`, `PAUSE`, `RESUME`.
  - Parking macros (parks the printhead at various locations): `PARKFRONT`, `PARKFRONTLOW`, `PARKREAR`, `PARKCENTER`, `PARKBED`.
  - Load/unload filament macros.
  - Purge line macro.

## To do:

- [x] Replace M109/M190 with `TEMPERATURE_WAIT`.
- [x] Get the Ellis `TEST_SPEED` macro working. ⚠️This works on Vorons only!.
- [x] Add information about directory structure.
- [x] Create FAQ section.
- [x] Get filament sensor working.
- [ ] Create topic in Discussion section detailing how users should keep this repository in sync with their own Klipper config using `git`.
- [ ] Explain `PAUSE`/`RESUME` extruder behaviour.
- [ ] Integrate KAMP (Klipper Adaptive Meshing and Purging).
- [x] Add `BEEP` when filament needs changing/`M600`.

## Stay Up-to-Date

I work on this repository all the time and a lot of new features are coming. Watch releases of this repository to be notified for future updates:

<img src="./images/githubstar.gif" width="500" alt='Raspberry Pi'/>

# Installation Steps

## Before You Begin

- Know what you're getting into by reading this documentation *fully!*
- It is assumed that you are connected to your host Raspberry Pi (or other host device) via SSH, and that your printer motherboard is connected to the host via USB.
- It is assumed that the username on the host device is `pi`. If that is not the case, you will have to manually edit `moonraker.conf` and `cfgs/misc-macros.cfg` and change any mentions of `/home/pi` to `/home/yourUserName`.
- It is assumed that you already have a working `printer.cfg` and you already have your printer up and running Klipper.
- Your question has probably been answered already, but if it hasn't, please post in the [Discussion](https://github.com/bassamanator/Sovol-SV06-firmware/discussions) section.
- If you see any errors, or encounter any issues, please create an [Issue](https://github.com/bassamanator/Sovol-SV06-firmware/issues/new), or a [Pull request](https://github.com/bassamanator/Sovol-SV06-firmware/pulls).
- I would recommend searching for the word `NOTE` in this repository. There are roughly half a dozen short points amongst the various files that you should be aware of if you're using this configuration.

## Download the Configuration

1. [Download](https://github.com/bassamanator/Sovol-SV06-firmware/archive/refs/heads/any-printer.zip) the `ZIP` file containing the Klipper configuration.
2. The parent folder in the `ZIP` is `Sovol-SV06-firmware-any-printer`. This is relevant in the next step.
3. Extract **only** the *contents* of the parent folder into `~/printer_data/config`.

💡 If you get a warning that you already have a `moonraker.conf` (which you probably do since you're already up and running Klipper), and *you're not using a low powered device such as the RPi Zero*, you need to simply paste the following into your *existing* `moonraker.conf`:
```
[file_manager]
enable_object_processing: True
```

## Setup Instructions

Simply add `[include ./osskc.cfg]` somewhere at the top of your `printer.cfg`.

## Directory Structure

This repository contains many files and folders. Some are *necessary* for this Klipper configuration to work, others are not.
- **Necessary** items are marked with a ✅.
- Items that can *optionally* be deleted are marked with a ❌.

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

## <img src="./misc/cup-border.png" width="30" alt='Ko-fi'/> Support Me <img src="./misc/cup-border.png" width="30" alt='Ko-fi'/>

<img src="./images/heart.gif" width="17" alt=''/> If you found my work useful, please consider buying me a [<img src="./misc/logo_white_stroke.png" height="20" alt='Ko-fi'/>](https://ko-fi.com/bassamanator).

## FAQ

##### How do I import a SuperSlicer configuration bundle (`SuperSlicer_config_bundle.ini`) into SuperSlicer?

Please see [this discussion](https://github.com/bassamanator/Sovol-SV06-firmware/discussions/13).

##### How do I print using SuperSlicer?

Please see [this discussion](https://github.com/bassamanator/Sovol-SV06-firmware/discussions/14).

##### When does beeping occur?

If you have a beeper pin set in your `printer.cfg`, the printer will beep upon:
- Filament runout.
- Filament change/`M600`.
- Upon `PRINT_END`.
- `MECHANICAL_GANTRY_CALIBRATION`/`G34`.

##### How do I disable/enable beeping?

Make the following changes according to your needs.

| File | `cfgs/misc-macros.cfg` |
| - | - |
| Section | `[gcode_macro _globals]` |
| Variable | `variable_beeping_enabled` |
| Disable beeping | `0` |
| Enable beeping | `1` |

##### I have a simple filament sensor connected. How do I enable/disable it?

Make the following changes according to your needs. Also, search for the word `NOTE` in this repository to find other places where such a sensor might come into play.

| File | `cfgs/misc-macros.cfg` |
| - | - |
| Section | `[gcode_macro _globals]` |
| Variable | `variable_filament_sensor_enabled` |
| Disable sensor | `0` |
| Enable sensor | `1` |

##### My filament runout sensor works, but I just started a print without any filament loaded. What gives?

A simple runout sensor can only detect a change in state. So, if you start a print without filament loaded, the printer will not know that there is no filament loaded. You should test your sensor by having filament loaded, starting a print, then cutting the filament. The expected behaviour is that the print will pause, and as long as you have beeping enabled, you will hear 3 annoying beeps.

## Useful Resources

- [RP2040-Zero ADXL345 Connection Klipper](https://github.com/bassamanator/rp2040-zero-adxl345-klipper)
- ⭐⭐⭐ [Ellis' Print Tuning Guide](https://ellis3dp.com/Print-Tuning-Guide)

## Sources

- https://www.klipper3d.org
- https://ellis3dp.com/Print-Tuning-Guide
- https://github.com/strayr/strayr-k-macros
- https://docs.vorondesign.com/build/software/miniE3_v20_klipper.html
- ⭐ https://github.com/spinixguy/Sovol-SV06-firmware
- https://www.printables.com/model/378915-sovol-sv06-buildplate-texture-and-model-for-prusas
- https://github.com/AndrewEllis93/Ellis-SuperSlicer-Profiles

[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/H2H0HIHTH)
