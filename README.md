# 🚨 *One-Stop-Shop* Sovol SV06 Klipper Configuration

## This branch contains the Klipper configuration and firmware for the Sovol SV06 3D printer with the `BTT SKR-Mini-E3-V3.0` motherboard.

## If you were looking for my OSS Klipper Configuration for the Sovol SV06 with *completely stock hardware*, please refer to the [master](https://github.com/bassamanator/Sovol-SV06-firmware/tree/master) branch.

## I am creating these files for my personal use and cannot be held responsible for what it might do to your printer.

## ❗☠️ I do not own this board so everything found on this branch is COMPLETELY UNTESTED ☠️❗
## ❗☠️ USE AT YOUR OWN RISK. YOU HAVE BEEN WARNED. ☠️❗

## An Important Note

⭐ A big thank you to Github user `transistor1`. This branch would not have been possible without his help. Please checkout his profile [here](https://github.com/transistor1). ⭐

## Installation Instructions

### Electronic Wiring

Wire cables according to the following diagram:
<img src="./misc/skr-mini/skr-mini-e3-v3.0-v1678580296384.png" alt='skr-mini-e3-v3.0 installation instructions'/>
### Download Firmware Precompiled by BIGTREETECH
- [firmware-USB.bin](./misc/skr-mini/firmware-USB.bin). Connect the SKR-Mini to the Raspberry Pi via USB connection.
- [firmware-USART2.bin](./misc/skr-mini/firmware-USART2.bin). Use TFT port USART2 to communicate with raspberry pi. Connect the UART-TX of raspberry pi with the USART-RX2 of motherboard and connect the UART-RX of raspberry pi with the USART-TX2 of motherboard directly to communicate normally.

Alternatively, find instructions on how to build the firmware yourself [here](https://github.com/bigtreetech/BIGTREETECH-SKR-mini-E3/tree/master/firmware/V3.0/Klipper#build-firmware-image).

### Flash Firmware

1. Rename `firmware-USB.bin` or `firmware-USART2.bin` to `firmware.bin`.
Important: If the file is not renamed, the bootloader will not be updated properly.
2. Copy `firmware.bin` to the root directory of the microSD card (make sure the card is in FAT32 format).
3. Power off the SKR-mini-E3-V3.0.
4. Insert the microSD card.
5. Power on the SKR-mini-E3-V3.0.
6. After a few seconds, the SKR-mini-E3-V3.0 should be flashed.
7. You can confirm that the flash was successful, by running ls /dev/serial/by-id. If the flash was successful, you'll see something like the following:
<img src="./misc/skr-mini/ls-output.png" alt='ls output'/>

### Download Klipper Configuration

1. Download [printer-skr-mini-e3-v3.cfg](./printer-skr-mini-e3-v3.cfg) and rename it to `printer.cfg`. This is the *only* `printer.cfg` that you will be using. Anytime there is a reference to `printer.cfg`, you will use this `printer.cfg` that you just saved/renamed.
2. Follow the instructions found in the [Download Klipper Configuration](https://github.com/bassamanator/Sovol-SV06-firmware#download-klipper-configuration) section and onwards. 💡 *Remember to use the `printer.cfg` that you saved in step 1*.

# Sources

- https://github.com/bigtreetech/BIGTREETECH-SKR-mini-E3/tree/master/firmware/V3.0/Klipper#how-to-use-klipper-on-skr-mini-e3-v30
- https://forum.sovol3d.com/t/sv06-mit-skr-e3-mini-v3/1189/24
- https://github.com/bassamanator/everything-sovol-sv06/discussions/14