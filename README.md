# 🚨 _One-Stop-Shop_ Sovol SV06 Klipper Configuration

This branch contains the Klipper configuration and firmware for the **Sovol SV06** 3D printer with the **BTT SKR-Mini-E3-V3.0** motherboard.

| Printer                     | Branch                                                                              |
| --------------------------- | ----------------------------------------------------------------------------------- |
| Sovol SV06                  | [master](https://github.com/bassamanator/Sovol-SV06-firmware/tree/master)           |
| Sovol SV06 Skr-Mini-E3-V3.0 | **You are here**                                                                    |
| Sovol SV06 Plus             | [sv06-plus](https://github.com/bassamanator/Sovol-SV06-firmware/tree/sv06-plus)     |
| All other printers          | [any-printer](https://github.com/bassamanator/Sovol-SV06-firmware/tree/any-printer) |

I am creating these files for my personal use and cannot be held responsible for what it might do to your printer. Use at your own risk.

🙏🏻🙌🏻 A big thanks to [transistor1](https://github.com/transistor1) for getting this `printer.cfg` up an running to begin with.

## Installation Instructions

### Electronic Wiring

Wire cables according to the following diagram:
<img src="./misc/skr-mini/skr-mini-e3-v3.0-v1678580296384.png" alt='skr-mini-e3-v3.0 installation instructions'/>

### Download Firmware

💡 The firmware files are located in `misc/skr-mini`.
💡 Be sure to download the `raw` files. Find out more about this [here](https://stackoverflow.com/questions/4604663/download-single-files-from-github).

Download [klipper-USB.bin](./misc/skr-mini/klipper-USB.bin) compiled by BTT.

Or you can download [klipper-v0.11.0-198-g33b18fd6-latest-UNTESTED.bin](./misc/skr-mini/klipper-v0.11.0-198-g33b18fd6-latest-UNTESTED.bin) that I compiled, however, it is **untested**.

#### Alternatively, find instructions on how to build the firmware yourself [here](https://github.com/bigtreetech/BIGTREETECH-SKR-mini-E3/tree/master/firmware/V3.0/Klipper#build-firmware-image)

<img src="./misc/skr-mini/menuconfig.png" width="500" alt='ls output'/>

### Flash Firmware

⚠️ Make sure the microSD card has been prepared using the instructions found [here](https://github.com/bassamanator/Sovol-SV06-firmware#prepare-the-microsd-card-for-flashing).

1. Rename whatever firmware file you're using to `firmware.bin`. ⚠️ If the file is not renamed, the bootloader will not be updated properly.
2. Copy `firmware.bin` to the root directory of the microSD card.
3. Power off the skr-mini-E3-V3.0.
4. Insert the microSD card.
5. Power on the skr-mini-E3-V3.0.
6. After a few seconds, the skr-mini-E3-V3.0 should be flashed, but wait a full minute just to be sure.
7. You can confirm that the flash was successful, by running `ls -l /dev/serial/by-id/` or `ls -l /dev/serial/by-path/`. If the flash was successful, you'll see something like the following:
   <img src="./misc/skr-mini/ls-output.png" width="400" alt='ls output'/>

### Download Klipper Configuration

You can choose _either_ of the 2 following methods.

### Method 1: Clone the Repository

1. `cd ~/printer_data/config`
2. Empty entire `~/printer_data/config` folder. Unfortunately, for safety reasons I will not post this command here. However, in linux, you can delete files via `rm filename`.
3. `git clone -b skr-mini-e3-v3 --single-branch https://github.com/bassamanator/Sovol-SV06-firmware.git .`

### Method 2: Download the ZIP

1. [Download](https://github.com/bassamanator/Sovol-SV06-firmware/archive/refs/heads/skr-mini-e3-v3.zip) the `ZIP` file containing the Klipper configuration.
2. See Step 2 in Method 1.
3. The parent folder in the `ZIP` is `Sovol-SV06-firmware-skr-mini-e3-v3`. This is relevant in the next step.
4. Extract **only** the _contents_ of the parent folder into `~/printer_data/config`.

### ⚠️ Important Note About Stepper Motor Current

For the SKR-Mini-E3-V3.0, the `run_current` for x, y, z stepper motors has been reduced in the `printer.cfg`, because the standard current seems to make the steppers dangerously hot. You may have to increase the current, or if the motors are still too hot, you may have to decrease it.

💡 Although stepper motors can withstand upwards of 125C, they should at the most get hot to the touch (~55C), not _very hot_.

#### Once you've cloned or downloaded the configuration, please follow instructions found in [Initial Steps](https://github.com/bassamanator/Sovol-SV06-firmware#initial-steps).

## <img src="./images/cup-border.png" width="30" alt='Ko-fi'/> Support Me <img src="./images/cup-border.png" width="30" alt='Ko-fi'/>

<img src="./images/heart.gif" width="17" alt=''/> If you found my work useful, please consider buying me a [<img src="./images/logo_white_stroke.png" height="20" alt='Ko-fi'/>](https://ko-fi.com/bassamanator).

# Sources

- https://github.com/bigtreetech/BIGTREETECH-SKR-mini-E3/tree/master/firmware/V3.0/Klipper#how-to-use-klipper-on-skr-mini-e3-v30
- https://forum.sovol3d.com/t/sv06-mit-skr-e3-mini-v3/1189/24
- https://github.com/bassamanator/everything-sovol-sv06/discussions/14
