<p align="center">
Please consider
<a href="https://ko-fi.com/bassamanator" target="_blank">donating</a> to
support my open source work ❤️
</p>

# One-Stop-Shop Klipper Configuration

| Printer                                                       | Branch                                                                                    |
| ------------------------------------------------------------- | ----------------------------------------------------------------------------------------- |
| ${\normalsize{\textcolor{darkturquoise}{\text{Sovol SV06}}}}$ | ⚡ ${\scriptsize{\textcolor{darkturquoise}{\text{YOU ARE HERE}}}}$ ⚡                     |
| Sovol SV06 SKR-Mini-E3-V3.0                                   | [skr-mini-e3-v3](https://github.com/bassamanator/Sovol-SV06-firmware/tree/skr-mini-e3-v3) |
| Sovol SV06 Fly-E3-Pro-V3                                      | [fly-e3-pro-v3](https://github.com/ElPainis/Fly-E3-Pro-v3) \*\*                           |
| Sovol SV06 Plus                                               | [sv06-plus](https://github.com/bassamanator/Sovol-SV06-firmware/tree/sv06-plus)           |
| All other printers                                            | [any-printer](https://github.com/bassamanator/Sovol-SV06-firmware/tree/any-printer)       |

${\small{\textit{** Maintained by ElPainis}}}$

> [!WARNING]
> I am creating these files for my personal use and cannot be held responsible for what it might do to your printer. Use at your own risk.

## Outline

- [Features](#features)
- [Stay Up-to-Date](#stay-up-to-date)
- [Preface](#preface)
- [Before You Begin](#before-you-begin)
- [Klipper Installation](#klipper-installation)
  - [Flash Firmware](#flash-firmware)
  - [Download OSS Klipper Configuration](#download-oss-klipper-configuration)
- [Initial Steps](#initial-steps)
  1. [Adjust Configuration with MCU Path](#adjust-configuration-with-mcu-path)
  2. [Configure Your Printer](#configure-your-printer)
- [Adjust Your Slicer](#adjust-your-slicer)
- [Support Me](#support-me)
- [Directory Structure](#directory-structure)
- [Special Considerations](#special-considerations)
- [FAQ](#faq)
- [Useful Resources](#useful-resources)
- [Sovol Official Links](#sovol-official-links)
- [Sources](#sources)

## Features

- 💥 This Klipper configuration is an _endpoint_, meaning that it contains **everything** that you could possibly need in order to have an excellent Klipper experience! 💥
- Filament runout sensor usage implemented.
- Minimum configuration settings for `Mainsail` and `Fluidd`.
- Pre-configured configuration bundles based on the [Ellis SuperSlicer Print Profiles](https://github.com/AndrewEllis93/Ellis-SuperSlicer-Profiles):
  - SuperSlicer
  - PrusaSlicer
  - OrcaSlicer (experimental, converted from PrusaSlicer)
  - Printer profiles: `SV06/Plus`; `SV07/Plus`
- Bed model and texture to use in SuperSlicer/PrusaSlicer.
- Macros:
  - **Improved** mechanical gantry calibration/`G34` macro that provides the user audio feedback, and time to check the calibration.
  - Misc macros: `PRINT_START`, `CANCEL_PRINT`, `PRINT_END`, `PAUSE`, `RESUME`.
  - Parking macros (parks the printhead at various locations): `PARKFRONT`, `PARKFRONTLOW`, `PARKREAR`, `PARKCENTER`, `PARKBED`.
  - Load/unload filament macros.
  - `PURGE_LINE` macro.
  - `TEST_SPEED` macro. Find instructions [here](#how-do-i-use-the-test_speed-macro).
- Klipper Adaptive Meshing & Purging (KAMP) integrated. Read about it [here](#what-do-i-need-to-know-about-kamp).

[🔼 Back to top](#outline)

## Stay Up-to-Date

${\normalsize{\textcolor{goldenrod}{\texttt{Star ⭐ this project.}}}}$

Watch for [updates](https://github.com/bassamanator/Sovol-SV06-firmware/discussions/37).

<img src="./misc/images/githubstar.gif" width="500" alt='github star'/>

[🔼 Back to top](#outline)

## Preface

Although I've made switching over to Klipper as easy as is possible, it can still be a challenge for some, especially considering that most of you have likely never used GNU+Linux. Save yourself the frustration, and fully read this documentation. Also note that Klipper is not a _must_, and is not for everyone. You can stick with Marlin, and have a fine 3D printing experience.

In many ways, this entire repository can be considered _my opinion_ on the `3D printing experience` and this has been _my attempt_ to share that experience. Some factors, such as _accuracy_ and _testing_, have been at the forefront of my thoughts during this process. I hope you find this repository suitable. Cheers.

[🔼 Back to top](#outline)

## Before You Begin

- This entire page is a **9 minute read**. Save yourself _hours of troubleshooting_ and read this documentation fully.
- Follow the steps in order. If an error was reported at a step, do no proceed to the next step.
- It is assumed that you are connected to your host Raspberry Pi (or other host device) via SSH, and that your printer motherboard is connected to the host via a _data_ USB cable.
- It is also assumed that the username on the host device is `pi`. If that is not the case, edit `moonraker.conf` and `cfgs/misc-macros.cfg` to change any mentions of `/home/pi` to `/home/yourUserName`.
- Klipper _must_ be installed on the host beforehand. Easiest is to use [MainsailOS](https://github.com/mainsail-crew/mainsail/releases/latest). [KIAUH](https://github.com/th33xitus/kiauh) is another option.
- Klipper _must_ be up to date.
  - In `Fluidd`, you can do this from `Settings` > `Software Updates`.
  - In `Mainsail`, you can do this from `Machine` > `Update Manager`.
- Robert Redford's performance in _Spy Game (2001)_ was superb!
- It is assumed that there is one instance of Klipper installed. If that is not the case, the steps in this guide will not work _perfectly_ for you.
- Your question has probably been answered already, but if it hasn't, please post in the [Discussion](https://github.com/bassamanator/Sovol-SV06-firmware/discussions) section.
- I would recommend searching for the word `NOTE` in this configuration. There are roughly half a dozen short points amongst the various files that you should be aware of.
- Consider [these](https://github.com/bassamanator/everything-sovol-sv06/blob/main/parts/README.md#printed-upgrades) printable parts, and also see my [Printables](https://www.printables.com/@bassamanator) page.

> [!TIP]
> Most of the micro USB cables that you find at home are _unlikely_ to be data cables, and it is not possible to tell just by looking.

> [!CAUTION]
> Make sure your printer is in good physical condition, because print and travel speeds will be _a lot faster_. Beginners would be wise to run through [these steps](https://github.com/bassamanator/everything-sovol-sv06/blob/main/initialsteps.md).

> [!CAUTION]
> [Disable](https://github.com/bassamanator/everything-sovol-sv06/blob/main/howto.md#disable-usb-cable-5v-pin) the USB cable's 5V pin.

[🔼 Back to top](#outline)

## Klipper Installation

### Flash Firmware

💡 If you flashed Klipper onto your motherboard in the past, you can skip this step.

Please note:

- For the sake of simplicity, I will refer to the firmware file as `klipper.bin` even though the actual filename is something along the lines of `klipper-v0.11.0-148-g52f4e20c.bin`.
- The firmware file is located in the `misc` folder.
- Flashing will only work if current firmware filename is _different from previous flashing procedure_. The `.bin` is also important.
- You may find this [video](https://youtu.be/p6l253OJa34) useful.

> [!WARNING]
> Many users have reported having issues flashing Klipper using the Sovol microSD card.

#### 1. Prepare the microSD Card for Flashing with These Parameters

- Size: `16GB` maximum.
- File system: `FAT32`.
- Allocation unit size: `4096 bytes`.
- Must not contain any files _except_ the firmware file.

#### 2. Flashing Procedure

1. Disconnect any USB cables that might be connected to the motherboard.
2. Copy `klipper.bin` to the microSD card.
3. Make sure the printer is off.
4. Insert the microSD card into printer.
5. Turn on the printer and wait a minute (usually takes 10 seconds).
6. Turn off the printer and remove the microSD.

> [!IMPORTANT]
> ⏲️ At this point, it's not possible to tell with certainty whether your flash was successful, continue on with the guide.

[🔼 Back to top](#outline)

### Download OSS Klipper Configuration

#### Method 1: Clone the Repository

💡 Make sure `git` is installed (`sudo apt update && sudo apt install git`).

1. `cd ~/printer_data/config`
2. Empty entire `~/printer_data/config` folder.
   - In linux, you can delete files via `rm fileName` and directories via `rmdir directoryName`.
   - In linux, you can list files and folders via `ls -lah`.
3. `git clone -b master --single-branch https://github.com/bassamanator/Sovol-SV06-firmware.git .` ⚠️ Don't miss the period!

#### Method 2: Download the ZIP

1. [Download](https://github.com/bassamanator/Sovol-SV06-firmware/archive/refs/heads/master.zip) the `ZIP` file containing the Klipper configuration.
2. See `Step 2` in `Method 1`.
3. The parent folder in the `ZIP` is `Sovol-SV06-firmware-master`. This is relevant in the next step.
4. Extract **only** the _contents_ of the parent folder into `~/printer_data/config`.

[🔼 Back to top](#outline)

## Initial Steps

### Adjust Configuration with MCU Path

💡 Make sure the host and printer are connected via USB.

1. Find what port the `mcu` (printer motherboard) is connected to via _one_ of the following commands:

   - `ls /dev/serial/by-id/*`
   - `ls /dev/serial/by-path/*`

   1. The output will be something along the lines of
      - `/dev/serial/by-id/usb-1a86_USB2.0-Serial-if00-port0`

2. Adjust the `[mcu]` section in `printer.cfg` accordingly.

   ```yaml
   # 📝 This is just an example
   [mcu]
   serial: /dev/serial/by-id/usb-1a86_USB2.0-Serial-if00-port0
   restart_method: command
   ```

3. Do a `FIRMWARE_RESTART`.

If the Klipper flash that you did earlier was successful, and you've done everything else correctly, you should see no errors or warnings in the `Mainsail`/`Fluidd` dashboard. 🎉 **Your printer has been Klipperized!** 🎉

[🔼 Back to top](#outline)

### Configure Your Printer

❗☠️ **Your finger should be on the power switch for most of these steps** ☠️❗

❗☠️ **Power off if there is a collision/problem** ☠️❗

💡 The ${\small{\textcolor{red}{\texttt{EMERGENCY STOP}}}}$ button in your dashboard works faster than hitting the power switch.

💡 Do a practice emergency stop.

💡 I recommend no filament be loaded for any of these steps.

> [!NOTE]
> You will be pasting/typing these commands into the `Mainsail`/`Fluidd` console.

1. Check to see if `X` and `Y` max positions can be reached, and adjust `position_max`, if necessary. You might be able to go further, which is great, but I recommend leaving a 2mm gap for safety.
   1. `G28`
   2. `G90`
   3. `G1 X223 F3000`
   4. `G1 Y223 F3000`
2. Do a mechanical gantry calibration; `G34`. After the controlled collision against the beam at the top, there will be a 10 second pause for you to verify that both sides of the gantry are pressed up against the `stoppers` at the top. You will hear a succession of beeps.
   1. Figure out your `Z` `position_max` by baby stepping your way up to the beam, and adjust `position_max`, if necessary.
3. PID tune the bed. Ideally, all PID tuning should occur at the temperatures that you print most at.
   1. `PID_TEST_BED TEMP=70`
   2. `SAVE_CONFIG` (once completed)
4. PID tune the extruder while part cooling fan runs at 25%.
   1. `PID_TEST_HOTEND TEMP=245`
   2. `SAVE_CONFIG` (once completed)
5. Adjust `z_offset`. Make sure your nozzle if very clean. Do the [Paper test](https://www.klipper3d.org/Bed_Level.html?h=probe_calibrate#the-paper-test).
   1. `DO_PROBE_CALIBRATE`
   2. Follow `z_offset` setup in `Mainsail`/`Fluidd`.
   3. `SAVE_CONFIG` (once completed)
6. Create a bed mesh.
   1. `DO_CREATE_MESH`
   2. `SAVE_CONFIG` (once completed)

🏁 If you've made it here, then your Klipperized printer is ready to print! 🏁

_But first_, adjust your slicer.

[🔼 Back to top](#outline)

## Adjust Your Slicer

> [!NOTE]
> If you are using the slicer bundles found on this repo, you can skip this section.

### Start G-Code

It varies depending on your slicer. Find instructions [here](https://ellis3dp.com/Print-Tuning-Guide/articles/passing_slicer_variables.html#slicer-start-g-code).

### End G-Code

```
PRINT_END
```

### Line Purge

If you would like to print a purge line before your print starts, at the end of your start gcode, on a new line, add one of the following:

- `PURGE_LINE`; prints a standard purge line.
- `LINE_PURGE`; prints KAMP's purge line.

> [!WARNING]
> Do not attempt to use `LINE_PURGE` without reading [this section](#what-do-i-need-to-know-about-kamp).

```yaml
# 📝 This is just an example Start G-Code
PRINT_START ...
PURGE_LINE
```

[🔼 Back to top](#outline)

## Support Me

Please ⭐ star this repository!

Support [open source](https://en.wikipedia.org/wiki/Open_source), and buy me a [<img src="./misc/images/logo_white_stroke.png" height="20" alt='Ko-fi'/>](https://ko-fi.com/bassamanator).

[🔼 Back to top](#outline)

## Directory Structure

This repository contains many files and folders. Some are _necessary_ for this Klipper configuration to work, others are not.

- **Necessary** items are marked with a ✅.
- Items that can _optionally_ be deleted are marked with a 💠.
<!-- tree -a -C -I '.directory' -L 1 -F -->

```sh
/home/pi/printer_data/config
├── cfgs/ ✅
├── CODE_OF_CONDUCT.md 💠
├── CONTRIBUTING.md 💠
├── .git/ ✅❔
├── .github/ 💠
├── .gitignore ✅❔
├── LICENSE 💠
├── misc/ 💠
├── moonraker.conf ✅
├── osskc.cfg ✅
├── printer.cfg ✅
├── README.md 💠
├── SECURITY.md 💠
└── .vscode/ 💠
```

[🔼 Back to top](#outline)

## Special Considerations

### Sequential printing

If enabled, cancelling, or resuming a print from pause, could lead to collisions with previously printed objects. In order to prevent collisions, in your slicer, ensure that objects are printed from the back of the build plate to the front.

In PrusaSlicer, please see Printer Settings > Notes, for extruder clearances.

### Renamed GCODE Commands

#### BED_MESH_CALIBRATE

Renamed to `_BED_MESH_CALIBRATE`.

### Errors

#### MCU 'mcu' shutdown: Timer too close

This error often occurs when the `mcu` is unable to generate the required `microsteps`. Lower power Klipper hosts might be especially susceptible. Reducing `microsteps` to `64`, or even `32` can resolve the issue.

[🔼 Back to top](#outline)

## FAQ

### What are some settings that I can change?

Edit the relevant file according to your needs.

| File                   | Section                  |
| ---------------------- | ------------------------ |
| `cfgs/misc-macros.cfg` | `[gcode_macro _globals]` |

| Variable                           | Disable       | Enable        | Notes                                          |
| ---------------------------------- | ------------- | ------------- | ---------------------------------------------- |
| `variable_beeping_enabled`         | `0`           | `1` (default) |
| `variable_filament_sensor_enabled` | `0` (default) | `1`           |
| `variable_kamp_enable`             | `0` (default) | `1`           | See [here](#what-do-i-need-to-know-about-kamp) |

### How do I import a configuration bundle into SuperSlicer/PrusaSlicer?

Please see this [discussion](https://github.com/bassamanator/Sovol-SV06-firmware/discussions/13).

### How do I print using SuperSlicer/PrusaSlicer?

Please see this [discussion](https://github.com/bassamanator/Sovol-SV06-firmware/discussions/14).

### When does beeping occur?

The printer will beep upon:

- Filament runout.
- Filament change/`M600`.
- Upon `PRINT_END`.
- `MECHANICAL_GANTRY_CALIBRATION`/`G34`.

### I want to use a filament sensor. How do I set it up?

You can find information about the physical setup [here](https://github.com/bassamanator/everything-sovol-sv06#filament-sensor).

You can test the sensor via `QUERY_FILAMENT_SENSOR SENSOR=filament_sensor`.

### My filament runout sensor works, but I just started a print without any filament loaded. What gives?

A simple runout sensor can only detect a change in state. So, if you start a print without filament loaded, the printer will not know that there is no filament loaded. You should test your sensor by having filament loaded, starting a print, then cutting the filament. The expected behaviour is that the print will pause, and as long as you have beeping enabled, you will hear 3 annoying beeps.

### What happens when I put in `M600`/colour change at a certain layer?

1. The printer will beep 3 times (not annoyingly).
2. Printing will stop.
3. The printhead will park itself front center.
4. The hotend will turn off, but the bed will remain hot.

### What happens when I pause a print?

Same behaviour as `M600`/colour change _except_ there won't be any beeping.

### What happens when filament runs out?

_If_ you have a working filament sensor, the same behaviour as `M600`/colour change will occur _except_ the beeps will be fairly annoying.

### How do I resume a print after a colour change or filament runout?

> [!WARNING]
> Do not disable the stepper motors during this process!

The printhead is now parked front center waiting for you to insert filament. You will:

1. Heat up the hotend to the desired temperature.
   - Use your Klipper dashboard.
2. Purge (push) some filament through the nozzle.
   - Use your Klipper dashboard, and extrude maybe 50mm (for a colour change you probably want to extrude more).
   - OR, you can push some filament by hand _making sure to first disengage the extruder's spring loaded arm_.
3. Hit resume in your Klipper dashboard.

### What do I need to know about KAMP?

> [!WARNING]
> No KAMP functionality can be used on low-powered devices such as the Raspberry Pi Zero.

> [!WARNING]
> If KAMP is disabled, and there is no `default` mesh, `PRINT_START` will crash.

> [!IMPORTANT]
> The [Label objects setting](https://docs.mainsail.xyz/overview/features/exclude-objects#enable-the-label-objects-setting-in-your-slicer) in your slicer must be enabled for KAMP to work.

> [!NOTE]
> `LINE_PURGE` is useable _on appropriate devices_ even if KAMP is disabled.

This repo contains all the code from the KAMP repository, however, only the `adaptive meshing` and `LINE_PURGE` functionality of KAMP has been configured and tested for use. To enable other functionality, adjust `/cfgs/kamp/KAMP_Settings.cfg`.

Read [KAMP official docs](https://github.com/kyleisah/Klipper-Adaptive-Meshing-Purging) to learn more.

### How do I use the `TEST_SPEED` macro?

> [!WARNING]
> This is for advanced users only, with well oiled machines. You can cause serious damage to your printer if you're not careful.

Find full instructions [here](https://ellis3dp.com/Print-Tuning-Guide/articles/determining_max_speeds_accels.html).

Some tips:

- Before running with `ITERATIONS=40` with an untested speed/accel value, run with `ITERATIONS=1`.
- Pay close attention throughout the run, so that you can click ${\small{\textcolor{red}{\texttt{EMERGENCY STOP}}}}$ at a moment's notice.
- This macro will simply help you determine the maximum speed your printhead and bed can reliably move at, not necessarily print at. The bottleneck for my SV06, for example, is the 15mm/s^2 that the hotend maxes out at (well under 200mm/s actual print speed).

### How do I compile my own firmware?

Please see this [discussion](https://github.com/bassamanator/Sovol-SV06-firmware/discussions/111).

[🔼 Back to top](#outline)

## Useful Resources

- [Everything Sovol SV06](https://github.com/bassamanator/everything-sovol-sv06)
- [RP2040-Zero ADXL345 Connection Klipper](https://github.com/bassamanator/rp2040-zero-adxl345-klipper)
- ⭐⭐⭐⭐⭐ [Ellis' Print Tuning Guide](https://ellis3dp.com/Print-Tuning-Guide)
- [Simplify3D Print Quality Troubleshooting Guide](https://www.simplify3d.com/resources/print-quality-troubleshooting/)

[🔼 Back to top](#outline)

## Sovol Official Links

- [SV06 Marlin Source Code](https://github.com/Sovol3d/Sv06-Source-Code)
- [SV06 Models](https://github.com/Sovol3d/SV06-Fully-Open-Source)
- [SV06 Plus Marlin Source Code and Models](https://github.com/Sovol3d/SV06-PLUS)

[🔼 Back to top](#outline)

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

[🔼 Back to top](#outline)
