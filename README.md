# 🚨 _One-Stop-Shop_ Klipper Configuration

This branch contains the Klipper configuration and firmware for the **Sovol SV06** 3D printer with the **BTT SKR-Mini-E3-V3.0** motherboard.

| Printer                                                                          | Branch                                                                              |
| -------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| Sovol SV06                                                                       | [master](https://github.com/bassamanator/Sovol-SV06-firmware/tree/master)           |
| ${\normalsize{\textcolor{darkturquoise}{\texttt{Sovol SV06 Skr-Mini-E3-V3.0}}}}$ | ⚡ ${\small{\textcolor{darkturquoise}{\text{YOU ARE HERE}}}}$ ⚡                    |
| Sovol SV06 Plus                                                                  | [sv06-plus](https://github.com/bassamanator/Sovol-SV06-firmware/tree/sv06-plus)     |
| All other printers                                                               | [any-printer](https://github.com/bassamanator/Sovol-SV06-firmware/tree/any-printer) |

I am creating these files for my personal use and cannot be held responsible for what it might do to your printer. Use at your own risk.

🙏🏻🙌🏻 Big thanks to [transistor1](https://github.com/transistor1) for getting the `printer.cfg` up an running to begin with.

## Outline

- [Features](#features)
- [Stay Up-to-Date](#stay-up-to-date)
- [Preface](#preface)
- [Before You Begin](#before-you-begin)
- [Electronic Wiring](#electronic-wiring)
  - [⚠️ Important Note About Stepper Motor Current](#important-note-about-stepper-motor-current)
- [Klipper Installation](#klipper-installation)
  - [Flash Firmware](#flash-firmware)
  - [Download OSS Klipper Configuration](#download-oss-klipper-configuration)
- [Initial Steps](#initial-steps)
  1. [Adjust Configuration with MCU Path](#adjust-configuration-with-mcu-path)
  2. [Configure Your Printer](#configure-your-printer)
- [Adjust Your Slicer](#adjust-your-slicer)
- [Support Me](#support-me)
- [Directory Structure](#directory-structure)
- [FAQ](#faq)
- [Useful Resources](#useful-resources)
- [Sovol Official Links](#sovol-official-links)
- [Sources](#sources)

## Features

- 💥 This Klipper configuration is an _endpoint_, meaning that it contains **everything** that you could possibly need in order to have an excellent Klipper experience! 💥
- Filament runout sensor usage implemented.
- Minimum configuration settings for `Mainsail` and `Fluidd`.
- Pre-configured configuration bundles based on the [Ellis SuperSlicer Print Profiles](https://github.com/AndrewEllis93/Ellis-SuperSlicer-Profiles)
  - SuperSlicer
  - PrusaSlicer
  - Printer profiles: SV06; SV06 Plus
- Bed model and texture to use in SuperSlicer/PrusaSlicer.
- Macros
  - **Improved** mechanical gantry calibration/`G34` macro that provides the user audio feedback, and time to check the calibration.
  - Misc macros: `PRINT_START`, `CANCEL_PRINT`, `PRINT_END`, `PAUSE`, `RESUME`.
  - Parking macros (parks the printhead at various locations): `PARKFRONT`, `PARKFRONTLOW`, `PARKREAR`, `PARKCENTER`, `PARKBED`.
  - Load/unload filament macros.
  - `PURGE_LINE` macro.
  - `TEST_SPEED` macro. Find instructions [here](#how-do-i-use-the-test_speed-macro).
- Klipper Adaptive Meshing & Purging (KAMP) integrated. Read about it [here](#how-do-i-enable-kamp-klipper-adaptive-meshing--purging).

[🔼 Back to top](#outline)

## Stay Up-to-Date

${\normalsize{\textcolor{goldenrod}{\texttt{Star ⭐ this project}}}}$

Watch for releases and updates. Please see this [discussion](https://github.com/bassamanator/Sovol-SV06-firmware/discussions/37).

<img src="./images/githubstar.gif" width="500" alt='github star'/>

[🔼 Back to top](#outline)

## Preface

Although I've made switching over to Klipper as easy as is possible, it can still be a challenge for some, especially considering that most of you have likely never used GNU+Linux. Save yourself the frustration, and fully read this documentation. Also note that Klipper is not a _must_, and is not for everyone. You can stick with Marlin, and have a fine 3D printing experience.

In many ways, this entire repository can be considered _my opinion_ on the `3D printing experience` and this has been _my attempt_ to share that experience. Some factors, such as _accuracy_ and _testing_, have been at the forefront of my thoughts during this process. I hope you find this repository suitable. Cheers.

[🔼 Back to top](#outline)

## Before You Begin

- This entire page is a **13 minute read**. Save yourself _hours of troubleshooting_ and read this documentation fully.
- Make sure your printer is in good physical condition, because print and travel speeds will be _a lot faster_ than they were before. Beginner's would be wise to go through the steps mentioned [here](https://github.com/bassamanator/everything-sovol-sv06/blob/main/initialsteps.md). Consider yourself warned.
- Follow the steps in order.
- If an error was reported at a step, do no proceed to the next step.
- It is assumed that you are connected to your host Raspberry Pi (or other host device) via SSH, and that your printer motherboard is connected to the host via a data USB cable. 💡 Most of the micro USB cables that you find at home are _unlikely_ to be data cables, and it's not possible to tell just by looking.
- [Disable](https://github.com/bassamanator/everything-sovol-sv06/blob/main/howto.md#disable-usb-cable-5v-pin) the USB cable's 5V pin.
- It is also assumed that the username on the host device is `pi`. If that is not the case, you will have to manually edit `moonraker.conf` and `cfgs/misc-macros.cfg` and change any mentions of `/home/pi` to `/home/yourUserName`.
- Klipper _must_ be installed on the host Raspberry Pi for everything to work. Easiest is to use a [MainsailOS](https://github.com/mainsail-crew/mainsail/releases/latest) image. Alternatively, you can install `Fluidd` or `Mainsail` via [KIAUH](https://github.com/th33xitus/kiauh).
- Klipper _must_ be up to date.
  - In `Fluidd`, you can do this from `Settings` > `Software Updates`.
  - In `Mainsail`, you can do this from `Machine` > `Update Manager`.
- Robert Redford's performance in _Spy Game (2001)_ was superb!
- It is assumed that there is one instance of Klipper installed. If you have multiple instances of Klipper installed, via `KIAUH` for example, then this guide is not for you. You can still use all the configs of course, but the steps in this guide will likely not work for you.
- Your question has probably been answered already, but if it hasn't, please post in the [Discussion](https://github.com/bassamanator/Sovol-SV06-firmware/discussions) section.
- I would recommend searching for the word `NOTE` in this configuration. There are roughly half a dozen short points amongst the various files that you should be aware of.

[🔼 Back to top](#outline)

## Electronic Wiring

Wire cables according to the following diagram:
<img src="./misc/skr-mini/skr-mini-e3-v3.0-v1678580296384.png" alt='skr-mini-e3-v3.0 installation instructions'/>

### Important Note About Stepper Motor Current

For the SKR-Mini-E3-V3.0, the `run_current` for x, y, z stepper motors has been reduced in the `printer.cfg`, because the standard current seems to make the steppers dangerously hot. You may have to increase the current, or if the motors are still too hot, you may have to decrease it.

💡 Although stepper motors can withstand upwards of 125C, they should at the most get hot to the touch (~55C), not _very hot_.

[🔼 Back to top](#outline)

## Klipper Installation

### Flash Firmware

💡 If you have already flashed klipper onto your motherboard in the past, you can skip this step.

Please note:

- For the sake of simplicity, I will refer to the klipper firmware file as `firmware.bin` even though the actual filename is something along the lines of `firmware-v0.12.0-10-gea2f6bc0-UNTESTED.bin`.
- The firmware file **must** be named `firmware.bin`.
- The firmware file is located in `misc/skr-mini`.
- ⚠️ Many users have reported having issues flashing Klipper using the Sovol microSD card.

#### 1. Prepare the microSD Card for Flashing with These Parameters

- Size: `16GB` maximum.
- File system: `FAT32`.
- Allocation unit size: `4096 bytes`.
- Must not contain any files _except_ the firmware file.

#### 2. Flashing Procedure

1. Disconnect any USB cables that might be connected to the motherboard.
2. Copy `firmware.bin` to the microSD card.
3. Make sure the printer is off.
4. Insert the microSD card into printer.
5. Turn on the printer and wait a minute (usually takes 10 seconds).
6. Turn off the printer and remove the microSD.

⏲️ At this point, it's not possible to tell with certainty whether your flash was successful, continue on with the guide.

[🔼 Back to top](#outline)

### Download OSS Klipper Configuration

You can choose _either_ of the 2 following methods.

#### Method 1: Clone the Repository

1. `cd ~/printer_data/config`
2. Empty entire `~/printer_data/config` folder.
   - In linux, you can delete files via `rm fileName` and directories via `rmdir directoryName`.
   - In linux, you can list files and folders via `ls -lah`.
3. `git clone -b skr-mini-e3-v3 --single-branch https://github.com/bassamanator/Sovol-SV06-firmware.git .` ⚠️ Don't miss the period!

#### Method 2: Download the ZIP

1. [Download](https://github.com/bassamanator/Sovol-SV06-firmware/archive/refs/heads/skr-mini-e3-v3.zip) the `ZIP` file containing the Klipper configuration.
2. See Step 2 in Method 1.
3. The parent folder in the `ZIP` is `Sovol-SV06-firmware-skr-mini-e3-v3`. This is relevant in the next step.
4. Extract **only** the _contents_ of the parent folder into `~/printer_data/config`.

[🔼 Back to top](#outline)

## Initial Steps

### Adjust Configuration with MCU Path

1. Find what port the `mcu` (printer motherboard) is connected to via **one** of the following commands:

   - `ls /dev/serial/by-id/*`
   - `ls /dev/serial/by-path/*`

   1. The output will be something along the lines of
      - `/dev/serial/by-id/usb-1a86_USB2.0-Serial-if00-port0`

2. Adjust the `[mcu]` section in `printer.cfg` accordingly.

   ```yaml
   # 🗒️ This is just an example
   [mcu]
   serial: /dev/serial/by-id/usb-1a86_USB2.0-Serial-if00-port0
   restart_method: command
   ```

3. Do a `FIRMWARE_RESTART`.

If the Klipper flash that you did earlier was successful, and you've done everything else correctly, you should see no errors or warnings in the `Mainsail`/`Fluidd` dashboard. **Klipper has successfully been installed on your printer!**

[🔼 Back to top](#outline)

### Configure Your Printer

❗☠️ **Your finger should be on the power switch for most of these steps** ☠️❗

❗☠️ **Power off if there is a collision/problem** ☠️❗

💡 The ${\small{\textcolor{red}{\texttt{EMERGENCY STOP}}}}$ button in your dashboard works faster than hitting the power switch.

💡 Do a practice emergency stop.

💡 I recommend no filament be loaded for any of these steps.

🗒️ You will be pasting/typing these commands into the `Mainsail`/`Fluidd` console.

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

🏁 If you've made it here, then your printer has been Klipperized, and is ready to print! 🏁

But first, adjust your slicer.

[🔼 Back to top](#outline)

## Adjust Your Slicer

💡 If you are using the config bundles found on this repo, you can skip this step.

You need to adjust the start and end gcode in your slicer. The relevant macros are `PRINT_START` and `PRINT_END`. Find instructions [here](https://ellis3dp.com/Print-Tuning-Guide/articles/passing_slicer_variables.html#slicer-start-g-code).

### Line Purge

If you would like to print a purge line before your print starts, at the end of your start gcode, on a new line, add one of the following:

- `PURGE_LINE`; prints a standard purge line.
- `LINE_PURGE`; prints KAMP's purge line. ⚠️ Do not attempt to use without reading [this section](#how-do-i-enable-kamp-klipper-adaptive-meshing--purging).

```yaml
# 🗒️ This is just an example Start G-Code
PRINT_START ...
PURGE_LINE
```

[🔼 Back to top](#outline)

## Support Me

Please ⭐ star this repository!

If you found my work useful, buy me a [<img src="./images/logo_white_stroke.png" height="20" alt='Ko-fi'/>](https://ko-fi.com/bassamanator).

[🔼 Back to top](#outline)

## Directory Structure

This repository contains many files and folders. Some are _necessary_ for this Klipper configuration to work, others are not.

- **Necessary** items are marked with a ✅.
- Items that can _optionally_ be deleted are marked with a ❌.
<!-- tree -a -C -I '.git|.directory' -L 1 -F -->

```sh
├── cfgs/ ✅
├── CODE_OF_CONDUCT.md ❌
├── CONTRIBUTING.md ❌
├── .github/ ❌
├── .gitignore ❌
├── images/ ❌
├── LICENSE ❌
├── misc/ ❌
├── moonraker.conf ✅
├── osskc.cfg ✅
├── printer.cfg ✅
├── README.md ❌
├── SECURITY.md ❌
└── .vscode/ ❌
```

[🔼 Back to top](#outline)

## FAQ

### How do I import a configuration bundle into SuperSlicer/PrusaSlicer?

Please see this [discussion](https://github.com/bassamanator/Sovol-SV06-firmware/discussions/13).

### How do I print using SuperSlicer?

Please see this [discussion](https://github.com/bassamanator/Sovol-SV06-firmware/discussions/14).

### When does beeping occur?

The printer will beep upon:

- Filament runout.
- Filament change/`M600`.
- Upon `PRINT_END`.
- `MECHANICAL_GANTRY_CALIBRATION`/`G34`.

Adjust behaviour according to your needs.

| File     | `cfgs/misc-macros.cfg`     |
| -------- | -------------------------- |
| Section  | `[gcode_macro _globals]`   |
| Variable | `variable_beeping_enabled` |
| Disable  | `0`                        |
| Enable   | `1` (default)              |

### I want to use a filament sensor. How do I set it up?

You can find information about the physical setup [here](https://github.com/bassamanator/everything-sovol-sv06#filament-sensor).

Adjust behaviour according to your needs.

| File     | `cfgs/misc-macros.cfg`             |
| -------- | ---------------------------------- |
| Section  | `[gcode_macro _globals]`           |
| Variable | `variable_filament_sensor_enabled` |
| Disable  | `0` (default)                      |
| Enable   | `1`                                |

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

⚠️ Do not disable the stepper motors during this process!

The printhead is now parked front center waiting for you to insert filament. You will:

1. Heat up the hotend to the desired temperature.
   - Use your Klipper dashboard.
2. Purge (push) some filament through the nozzle.
   - Use your Klipper dashboard, and extrude maybe 50mm (for a colour change you probably want to extrude more).
   - OR, you can push some filament by hand _making sure to first disengage the extruder's spring loaded arm_.
3. Hit resume in your Klipper dashboard.

### How do I enable KAMP (Klipper Adaptive Meshing & Purging)?

⚠️ No KAMP functionality can be used on low-powered devices such as the Raspberry Pi Zero.

⚠️ If KAMP is disabled, and there is no `default` mesh, `PRINT_START` will crash.

🗒️ The [Label objects setting](https://docs.mainsail.xyz/overview/features/exclude-objects#enable-the-label-objects-setting-in-your-slicer) in your slicer must be enabled for KAMP to work.

🗒️ `LINE_PURGE` is useable (on appropriate devices) even if KAMP is disabled.

This repo contains all the code from the KAMP repository, however, only the `adaptive meshing` and `LINE_PURGE` functionality of KAMP has been configured and tested for use. To enable other functionality, adjust `/cfgs/kamp/KAMP_Settings.cfg`.

Adjust behaviour according to your needs.

| File     | `cfgs/misc-macros.cfg`   |
| -------- | ------------------------ |
| Section  | `[gcode_macro _globals]` |
| Variable | `variable_kamp_enable`   |
| Disable  | `0` (default)            |
| Enable   | `1`                      |

### How do I use the `TEST_SPEED` macro?

⚠️ This is for advanced users only, with well oiled machines. You can cause serious damage to your printer if you're not careful. ☠️ **You have been warned** ☠️.

Find full instructions [here](https://ellis3dp.com/Print-Tuning-Guide/articles/determining_max_speeds_accels.html).

Some tips:

- Before running with `ITERATIONS=40` with an untested speed/accel value, run with `ITERATIONS=1`.
- Pay close attention throughout the run, so that you can click ${\small{\textcolor{red}{\texttt{EMERGENCY STOP}}}}$ at a moment's notice.
- This macro will simply help you determine the maximum speed your printhead and bed can reliably move at, not necessarily print at. The bottleneck for my SV06, for example, is the 15mm/s^2 that the hotend maxes out at (well under 200mm/s actual print speed).

### How do I compile my own firmware?

Please see this [discussion](https://github.com/bassamanator/Sovol-SV06-firmware/discussions/117).

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
