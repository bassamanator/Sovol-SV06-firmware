<p align="center">
Please consider
<a href="https://ko-fi.com/bassamanator" target="_blank">donating</a> to
support my open source work ❤️
</p>

# One-Stop-Shop Klipper Configuration

| Printer                                                       | Branch                                                                                    |
| ------------------------------------------------------------- | ----------------------------------------------------------------------------------------- |
| ${\normalsize{\textcolor{darkturquoise}{\text{Sovol SV06}}}}$ | [master](https://github.com/bassamanator/Sovol-SV06-firmware/tree/master)                     |
| Sovol SV06 SKR-Mini-E3-V3.0                                   | [skr-mini-e3-v3](https://github.com/bassamanator/Sovol-SV06-firmware/tree/skr-mini-e3-v3) |
| Sovol SV06 Fly-E3-Pro-V3                                      | [fly-e3-pro-v3](https://github.com/ElPainis/Fly-E3-Pro-v3) \*\*                           |
| Sovol SV06 Plus                                               | [sv06-plus](https://github.com/bassamanator/Sovol-SV06-firmware/tree/sv06-plus)           |
| All other printers                                            | ⚡ ${\scriptsize{\textcolor{darkturquoise}{\text{YOU ARE HERE}}}}$ ⚡       |

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
  - [Setup Instructions](#setup-instructions)
- [Adjust Your Slicer](#adjust-your-slicer)
- [Support Me](#support-me)
- [Directory Structure](#directory-structure)
- [Special Considerations](#special-considerations)
- [FAQ](#faq)
- [Useful Resources](#useful-resources)
- [Sovol Official Links](#sovol-official-links)
- [Sources](#sources)

## Features

- 💥 This Klipper configuration is an _endpoint_, meaning that it contains **everything** that you could possibly need in order to have an excellent Klipper experience! 💥 CoreXY users can rightly disagree and say that it lacks the quad gantry levelling macros. Please create a pull request if you can help in this regard!
- Filament runout sensor usage implemented.
- Minimum configuration settings for `Mainsail` and `Fluidd`.
- Pre-configured configuration bundles based on the [Ellis SuperSlicer Print Profiles](https://github.com/AndrewEllis93/Ellis-SuperSlicer-Profiles):
  - SuperSlicer
  - PrusaSlicer
- Macros
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

- This entire page is a **12 minute read**. Save yourself _hours of troubleshooting_ and read this documentation fully.
- It is assumed that you already have a working `printer.cfg` and you already have your printer up and running Klipper.
- The [master](https://github.com/bassamanator/Sovol-SV06-firmware/tree/master) branch of this repo contains baby step by step instructions on how to get Klipper going for a particular bed printer (Sovol SV06). If you are a beginner, you might find those instructions useful.
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
- ~~It is assumed that there is one instance of Klipper installed. If you have multiple instances of Klipper installed, via `KIAUH` for example, then this guide is not for you. You can still use all the configs of course, but the steps in this guide will likely not work for you.~~
- Your question has probably been answered already, but if it hasn't, please post in the [Discussion](https://github.com/bassamanator/Sovol-SV06-firmware/discussions) section.
- I would recommend searching for the word `NOTE` in this configuration. There are roughly half a dozen short points amongst the various files that you should be aware of.
- You must have a `bed_mesh` for `PRINT_START` to work correctly. If you do not have a probe to create a `bed_mesh`, simply comment out the line `BED_MESH_PROFILE LOAD=default` in the `PRINT_START` macro.

[🔼 Back to top](#outline)

## Klipper Installation

### Flash Firmware

You must flash your motherboard according to the manufacturer's instructions.

### Download OSS Klipper Configuration

1. [Download](https://github.com/bassamanator/Sovol-SV06-firmware/archive/refs/heads/any-printer.zip) the `ZIP` file containing the Klipper configuration.
2. The parent folder in the `ZIP` is `Sovol-SV06-firmware-any-printer`. This is relevant in the next step.
3. Extract **only** the _contents_ of the parent folder into `~/printer_data/config`.

See what files are necessary and which ones can be skipped [here](#directory-structure).

💡 **If** you get a warning that you already have a `moonraker.conf` **and if** you're _not_ using a low powered device such as a Raspberry Pi Zero, do not overwrite the _existing_ `moonraker.conf` and simply paste the following into it:

```yaml
[file_manager]
enable_object_processing: True
```

### Setup Instructions

Simply add `[include ./osskc.cfg]` somewhere at the top of your `printer.cfg`.

And remember to _adjust your slicer_.

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
├── .github/ 💠
├── .gitignore 💠
├── images/ 💠
├── LICENSE 💠
├── misc/ 💠
├── moonraker.conf ✅ ❔
├── osskc.cfg ✅
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
| `variable_beeping_enabled`         | `0` (default) | `1`           |
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
