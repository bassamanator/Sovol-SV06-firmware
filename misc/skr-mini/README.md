# Klipper Firmware

## Firmware Precompiled by BIGTREETECH
- [firmware-USB.bin](./firmware-USB.bin). Connect the SKR-Mini to the Raspberry Pi via USB connection.
- [firmware-USART2.bin](./firmware-USART2.bin). Use TFT port USART2 to communicate with raspberry pi. Connect the UART-TX of raspberry pi with the USART-RX2 of motherboard and connect the UART-RX of raspberry pi with the USART-TX2 of motherboard directly to communicate normally.

Alternatively, find instructions on how to build the firmware yourself [here](https://github.com/bigtreetech/BIGTREETECH-SKR-mini-E3/tree/master/firmware/V3.0/Klipper#build-firmware-image).

## Flash Firmware

1. Rename `firmware-USB.bin` or `firmware-USART2.bin` to `firmware.bin`.
Important: If the file is not renamed, the bootloader will not be updated properly.
2. Copy `firmware.bin` to the root directory of the microSD card (make sure the card is in FAT32 format).
3. Power off the SKR-mini-E3-V3.0.
4. Insert the microSD card.
5. Power on the SKR-mini-E3-V3.0.
6. After a few seconds, the SKR-mini-E3-V3.0 should be flashed.
7. You can confirm that the flash was successful, by running ls /dev/serial/by-id. If the flash was successful, you'll see something like the following:
<img src="ls-output.png" alt='skr-mini-e3-v3.0 installation instructions'/>

# Electronic Wiring

<img src="skr-mini-e3-v3.0-v1678580296384.png" alt='skr-mini-e3-v3.0 installation instructions'/>

# Sources

- https://github.com/bigtreetech/BIGTREETECH-SKR-mini-E3/tree/master/firmware/V3.0/Klipper#how-to-use-klipper-on-skr-mini-e3-v30
- https://forum.sovol3d.com/t/sv06-mit-skr-e3-mini-v3/1189/24
