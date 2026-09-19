${\small{\textcolor{green}{\texttt{2 minute read}}}}$

# Everything about `klipper.Bin` for the SV06/Plus

## FAQs

### Do I need to re-flash the motherboard?

You will _almost never_ need to re-flash your motherboard. When this is needed, your dashboard will explicitly tell you that you need to re-flash.

### Where does Klipper live?

1. On the motherboard.
2. On the host device (Raspberry Pi, etc.).

🗒️ These 2 versions of klipper can rightly differ.

### Should I update Klipper, moonraker, etc.?

I always update everything on the host device via the dashboard. I don't want to miss out on improvements, especially those that improve safety. What this means is that on some occasions, there will be breaking changes: you will need to change a few things in your config **before you can print**. I would suggest that if you absolutely have to get something printed immediately, and there's an update, perhaps wait till after the print completes to do the update.

Others will argue that "if it ain't broke, don't fix it".

You have to decide what camp you want to be apart off.

#### How-To

- In `Fluidd`, you can do this from `Settings` > `Software Updates`.
- In `Mainsail`, you can do this from `Machine` > `Update Manager`.

## Compilation Steps

### SV06/Plus

1. `ssh` into the Klipper host (i.e., RPi, OrangePi, etc.).
2. `cd ~/klipper`
3. `git pull`
    - Pulls the latest changes from the klipper repo, ensuring that your `klipper.bin` will be as fresh as possible.
4. `make menuconfig`
5. Set things up to look as follows:

    ![make-menuconfig](https://github.com/bassamanator/Sovol-SV06-firmware/blob/51058eacde444c16d6a8a87db06d91568f040ec0/misc/images/make-menuconfig-latest.png)
6. `make clean`
    - Clears `~/klipper/out/`
7. `make`
    - Compiles `klipper.bin` and puts it in `~/klipper/out/`

### SKR-MINI-E3-V3

Follow the same steps as above except set things up to look as follows:

![make-menuconfig](https://github.com/bassamanator/Sovol-SV06-firmware/blob/ce233d2f6e615b690942cff995187a3c5923945b/misc/skr-mini/menuconfig.png)
