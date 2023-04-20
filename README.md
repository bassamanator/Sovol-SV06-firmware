# 🚨 *One-Stop-Shop* Sovol SV06 Klipper Configuration

This repository contains the Klipper configuration and firmware for the **Sovol SV06** 3D printer with completely *stock hardware*.

If you wanted to use the One-Stop-Shop Klipper Configuration for a *different printer*, please switch to the [any-printer](https://github.com/bassamanator/Sovol-SV06-firmware/tree/any-printer) branch.

I am creating these files for my personal use and cannot be held responsible for what it might do to your printer. Use at your own risk.

# Highlights

- 💥 This Klipper configuration is an *endpoint*, meaning that it contains **everything** that you could possibly need in order to have an excellent Klipper experience! 💥
- `NEW` <img src="./images/party_blob.gif" width="20" alt=''/> Filament runout sensor usage implemented. <img src="./images/party_blob.gif" width="20" alt=''/> 
- Minimum configuration settings for Mainsail/Fluiddpi to work.
- SuperSlicer config bundle that contains the printer configuration, as well as what are considered by many to be the best print settings available for any FDM printer ([Ellis' SuperSlicer Profiles](https://github.com/AndrewEllis93/Ellis-SuperSlicer-Profiles)). Find the differences between the different print setting profiles [here](https://github.com/AndrewEllis93/Ellis-SuperSlicer-Profiles/tree/master/SuperSlicer). But basically, the 45 degree profile places the seam at the back.
- Bed model and texture to use in SuperSlicer/PrusaSlicer.
- Macros
  - **Improved** mechanical gantry calibration/`G34` macro that provides the user audio feedback, and time to check the calibration.
  - Misc macros: `PRINT_START`, `CANCEL_PRINT`, `PRINT_END`, `PAUSE`, `RESUME`.
  - Parking macros (parks the printhead at various locations): `PARKFRONT`, `PARKFRONTLOW`, `PARKREAR`, `PARKCENTER`, `PARKBED`.
  - Load/unload filament macros.
  - Purge line macro.

## Stay Up-to-Date

I work on this repository all the time and a lot of new features are coming. Watch releases of this repository to be notified for future updates:

<img src="./images/githubstar.gif" width="500" alt='Raspberry Pi'/>

# Preface

Although I've made switching over to Klipper as easy as is possible, it can still be a challenge for some, especially considering that most of you have likely never used GNU+Linux. Save yourself the frustration, and fully read all documentation found on this page. Also note that Klipper is not a *must*, and is not for everyone. You can stick with Marlin, and have a fine 3D printing experience.

# Installation Steps

## Before You Begin

- Read this documentation *fully!*
- Make sure your printer is in good physical condition, because print and travel speeds will be *a lot faster* than they were before. Consider yourself warned.
- Follow the steps in order.
- If an error was reported at a step, do no proceed to the next step.
- It is assumed that you are connected to your host Raspberry Pi (or other host device) via SSH, and that your printer motherboard is connected to the host via a data USB cable. Note that most of the micro USB cables that you find at home are *unlikely* to be data cables, and it's not possible to tell just by looking.
- It is also assumed that the username on the host device is `pi`. If that is not the case, you will have to manually edit `moonraker.conf` and `cfgs/misc-macros.cfg` and change any mentions of `/home/pi` to `/home/yourUserName`.
- Klipper *must* be installed on the host Raspberry Pi for everything to work. Easiest is to use a [FluiddPi](https://docs.fluidd.xyz/installation/fluiddpi#download) or [MainsailOS](https://github.com/mainsail-crew/mainsail/releases/latest) image. [KIAUH](https://github.com/th33xitus/kiauh) is another excellent option, though it's not as simple as flashing MainsailOS on a microSD card, for example.
- Robert Redford's performance in *Spy Game (2001)* was superb!
- It is assumed that there is one instance of Klipper installed. If you have multiple instances of Klipper installed, via `KIAUH` for example, then this guide is not for you. You can still use all the configs of course, but the steps in this guide will likely not work for you.
- Your question has probably been answered already, but if it hasn't, please post in the [Discussion](https://github.com/bassamanator/Sovol-SV06-firmware/discussions) section.
- I would recommend searching for the word `NOTE` in this repository. There are roughly half a dozen short points amongst the various files that you should be aware of if you're using this configuration.

## Flash Firmware

💡 *If you have already flashed klipper onto your motherboard in the past, you can skip this step.*

💡 For the sake of simplicity, I will refer to the klipper firmware file as `klipper.bin` even though the actual filename is something along the lines of `klipper-v0.11.0-148-g52f4e20c.bin`.

💡 The firmware file is located in the `misc` folder.

### Prepare the microSD Card for Flashing

- Size: `8GB`. According to Sovol, the largest size that you can use is `16GB`.
- File system: `FAT32`.
- Must not contain any files *except* the firmware file.

### Flashing Procedure

1. Disconnect any USB cables that might be connected to the motherboard.
2. Copy `klipper.bin` to the microSD card.
3. Make sure the printer is off.
4. Insert the microSD card into printer.
4. Turn on the printer and wait a minute (usually takes 10 seconds).
5. Turn off the printer and remove the microSD.

You may find this [video](https://youtu.be/p6l253OJa34) useful.

⚠️ **Caveat**: Flashing will only work if current firmware filename is *different from previous flashing procedure*. The `.bin` is also important.

## Download Klipper Configuration

You can choose *either* of the 2 following methods.

### Method 1: Clone the Repository

1. `cd ~/printer_data/config`
2. Empty entire `~/printer_data/config` folder. Unfortunately, for safety reasons I will not post this command here. However, in linux, you can delete files via `rm filename`.
3. `git clone -b master --single-branch https://github.com/bassamanator/Sovol-SV06-firmware.git .`

### Method 2: Download the ZIP

1. [Download](https://github.com/bassamanator/Sovol-SV06-firmware/archive/refs/heads/master.zip) the `ZIP` file containing the Klipper configuration.
2. See `Step 2` in `Method 1`.
3. The parent folder in the `ZIP` is `Sovol-SV06-firmware-master`. This is relevant in the next step.
4. Extract **only** the *contents* of the parent folder into `~/printer_data/config`.

## Initial Steps

### Step 1

1. Find what port the `mcu` (SV06 motherboard) is connected to via `ls -l /dev/serial/by-id/`.
2. Adjust the `[mcu]` section in `printer.cfg` accordingly.

### Step 2

❗☠️ **Your finger should be on the power switch for most of these steps** ☠️❗

❗☠️ **Power off if there is a collision/problem** ☠️❗

💡 I recommend no filament be loaded for any of these steps.

💡 Find explanations for gcode commands at [https://marlinfw.org/](https://marlinfw.org/) and [klipper.org](https://www.klipper3d.org/G-Codes.html).

1. `G28`
   1. Check to see if `X` and `Y` max positions (`G90`, `G1 X223 F3000`, `G1 Y223 F3000`) can be reached, and adjust `position_max`, if necessary. You can probably go all the way up to `225` for `X` and `Y` both, however, I would not recommend it.
2. Do a `G34`; mechanical gantry calibration. After the controlled collision against the beam at the top, there will be a 10 second pause for you to verify that both sides of the gantry are pressed up against the `stoppers` at the top. You will hear a succession of beeps.
   1. Figure out your `Z` `position_max` by baby stepping your way up to the beam. The range is 250 to 261 from what I've seen, could be even higher for you. Adjust `position_max`, if necessary. I can go all the way to 258, however, I would not print anything higher than 257.
3. Pid tune the bed, but first move the printhead to the center. Ideally, all Pid tuning should occur at the temperatures that you print most at.
   1. `G28`
   2. `G90`
   3. `G1 X111 Y111 Z40 F6000`
   4. `PID_CALIBRATE HEATER=heater_bed TARGET=70`
   5. `SAVE_CONFIG` (once completed)
4. Pid tune the extruder while part cooling fan runs at 25%.
   1. `G28`
   2. `G90`
   3. `G1 X111 Y111 Z10 F6000`
   4. `M106 S64`
   5. `PID_CALIBRATE HEATER=extruder TARGET=245`
   6. `SAVE_CONFIG` (once completed)
5. Adjust `z_offset`. Make sure your nozzle if very clean. Do the [Paper test](https://www.klipper3d.org/Bed_Level.html?h=probe_calibrate#the-paper-test).
   1. `SET_HEATER_TEMPERATURE HEATER=heater_bed TARGET=60`
   2. `SET_HEATER_TEMPERATURE HEATER=extruder TARGET=180`
   3. Proceed to next steps after both temperatures have been reached.
   4. `G28`
   5. `PROBE_CALIBRATE`
   6. `SAVE_CONFIG` (once completed)
6. Create a bed mesh.
   1. `SET_HEATER_TEMPERATURE HEATER=heater_bed TARGET=60`
   2. `SET_HEATER_TEMPERATURE HEATER=extruder TARGET=180`
   3. Proceed to next steps after both temperatures have been reached.
   4. `G28`
   5. `BED_MESH_CALIBRATE`
   6. `SAVE_CONFIG` (once completed)

If you've made it here, then your printer has been Klipperized, and is ready to print!

But first, adjust your slicer.

## Adjust Your Slicer

You need to adjust the start and end gcode in your slicer. The relevant macros are `PRINT_START` and `PRINT_END`. Find instructions [here](https://ellis3dp.com/Print-Tuning-Guide/articles/passing_slicer_variables.html#slicer-start-g-code).

## Directory Structure

This repository contains many files and folders. Some are *necessary* for this Klipper configuration to work, others are not.
- **Necessary** items are marked with a ✅.
- Items that can *optionally* be deleted are marked with a ❌.

```
├── cfgs ✅
│   ├── adxl-direct.cfg
│   ├── adxl-rp2040.cfg
│   ├── beeper.cfg
│   ├── misc-macros.cfg
│   ├── MECHANICAL_GANTRY_CALIBRATION.cfg
│   ├── PARKING.cfg
│   └── TEST_SPEED.cfg [☠️Not ready for use☠️]
├── images ❌
│   └── githubstar.gif
├── misc ❌
│   ├── cup-border.png
│   ├── klipper.bin
│   ├── logo_white_stroke.png
│   ├── M503-output.yml
│   ├── SuperSlicer_config_bundle.ini
│   ├── sv06-buildPlate.png
│   ├── SV06-buildPlate.stl
│   └── SV06-texture.svg
├── moonraker.conf ✅
├── printer.cfg ✅
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

The printer will beep upon:
- Filament runout.
- Filament change/`M600`.
- Upon `PRINT_END`.
- `MECHANICAL_GANTRY_CALIBRATION`/`G34`.

##### How do I disable beeping?

Make the following changes according to your needs. All beeping will be disabled *except* during gantry calibration.

| File | `cfgs/misc-macros.cfg` |
| - | - |
| Section | `[gcode_macro _globals]` |
| Variable | `variable_beeping_enabled` |
| Disable beeping | `0` |
| Enable beeping | `1` |

##### I want to use a filament sensor. How do I set it up?

 You can find information about the physical setup [here](https://github.com/bassamanator/everything-sovol-sv06#filament-sensor).
##### I have a simple filament sensor connected. How do I enable/disable it?

Make the following changes according to your needs.

| File | `cfgs/misc-macros.cfg` |
| - | - |
| Section | `[gcode_macro _globals]` |
| Variable | `variable_filament_sensor_enabled` |
| Disable sensor | `0` |
| Enable sensor | `1` |

##### My filament runout sensor works, but I just started a print without any filament loaded. What gives?

A simple runout sensor can only detect a change in state. So, if you start a print without filament loaded, the printer will not know that there is no filament loaded. You should test your sensor by having filament loaded, starting a print, then cutting the filament. The expected behaviour is that the print will pause, and as long as you have beeping enabled, you will hear 3 annoying beeps.

##### What happens when I put in `M600`/colour change at a certain layer?

1. The printer will beep 3 times (not annoyingly).
2. Printing will stop.
3. The printhead will park itself front center.
4. The hotend will turn off, but the bed will remain hot.

##### What happens when I pause a print?

Same behaviour as `M600`/colour change *except* there won't be any beeping.

##### What happens when filament runs out?

*If* you have a working filament sensor, the same behaviour as `M600`/colour change will occur*except* the beeps will be fairly annoying.

##### How do I resume a print after a colour change or filament runout?

*Do no disable the stepper motors during this process!*

The printhead is now parked front center waiting for you to insert filament. You will:

1. Heat up the hotend to the desired temperature.
    - Use your Klipper dashboard.
2. Purge (push) some filament through the nozzle.
    - Use your Klipper dashboard, and extrude maybe 50mm (for a colour change you probably want to extrude more).
    - OR, you can push some filament by hand *making sure to first disengage the extruder's spring loaded arm*.
3. Hit resume in your Klipper dashboard.

## Useful Resources

- [Everything Sovol SV06](https://github.com/bassamanator/everything-sovol-sv06)
- [RP2040-Zero ADXL345 Connection Klipper](https://github.com/bassamanator/rp2040-zero-adxl345-klipper)
- ⭐⭐⭐⭐⭐ [Ellis' Print Tuning Guide](https://ellis3dp.com/Print-Tuning-Guide)
- [Simplify3D Print Quality Troubleshooting Guide](https://www.simplify3d.com/resources/print-quality-troubleshooting/)

## Links

- [SV06 Official Marlin Source Code](https://github.com/Sovol3d/Sv06-Source-Code)
- [SV06 Official Models](https://github.com/Sovol3d/SV06-Fully-Open-Source)

## Sources

- https://www.klipper3d.org
- https://ellis3dp.com/Print-Tuning-Guide
- https://github.com/strayr/strayr-k-macros
- https://docs.vorondesign.com/build/software/miniE3_v20_klipper.html
- https://github.com/spinixguy/Sovol-SV06-firmware
- https://www.printables.com/model/378915-sovol-sv06-buildplate-texture-and-model-for-prusas
- https://github.com/AndrewEllis93/Ellis-SuperSlicer-Profiles

[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/H2H0HIHTH)
