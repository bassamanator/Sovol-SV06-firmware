# Sovol-SV06-firmware

This repository contains firmware for the SV06 3D printer from Sovol. You should not use this firmware on any other printer.

Use at your own risk. I am creating these files for my personal use and cannot be held responsible for what it might do to your printer.

# Installation Steps

**Requirement**: klipper must be installed on the RPI. Easiest is to use a Fluiddpi or MailsailOS image.

## Step 1

### Build Firmware Image

SSH into the Raspberry Pi and run:

```
sudo apt install make
cd ~/klipper
make menuconfig
```

In the menu structure there are a number of items to be selected.

- Ensure that the processor model is set to ‘STM32F103’
- Ensure that the Bootloader offset is set to ‘28KiB’
- Ensure 'Disable SWD at startup' is set
- Set communication to serial (on USART1 PA10/PA9)

Once the configuration is selected, select “Exit” and “Yes” if asked to save the configuration.

Run the following:

```
make clean
make
```

## Step 2

### Flash Firmware

The `make` command, when completed, creates a firmware file `klipper.bin` which is stored in the folder `/home/pi/klipper/out`.

#### Method 1 (Perhaps more straightforward)

1. Copy `klipper.bin` to a MicroSD card and rename to `anyNewFilename.bin`.
2. Make sure printer is off.
3. Insert MicroSD into printer.
4. Turn on printer and wait a couple of minutes (usually takes under a minute).
5. Turn off printer and remove MicroSD.

**Caveat**: flashing will only work if current firmware filename (`anyNewFilename.bin` in this example) is different from previous flashing procedure. The `.bin` is also important.

#### Method 2 (Maybe a bit advanced)

Find port that the printer board is connected to your RPI via `ls /dev/serial/by-id/*`. It should report something similar to the following `/dev/serial/by-id/usb-1a86_USB2.0-Serial-if00-port0`. Run the following commands:

```
sudo service klipper stop
make flash FLASH_DEVICE=/dev/serial/by-id/usb-1a86_USB2.0-Serial-if00-port0
sudo service klipper start
```

## Sources

- https://docs.vorondesign.com/build/software/miniE3_v20_klipper.html
- https://www.klipper3d.org/RPi_microcontroller.html#rpi-microcontroller
