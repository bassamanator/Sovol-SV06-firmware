${\small{\textcolor{green}{\texttt{2 minute read}}}}$

#### Do I need to re-flash the motherboard?

You will _almost never_ need to re-flash `klipper.bin`. When this is needed, your dashboard will explicitly tell you that you need to re-flash.

#### Where does Klipper live?

1. On the motherboard.
2. On the host device (Raspberry Pi, etc.).

🗒️ These 2 versions of klipper can rightly differ.

#### Should I update Klipper, moonraker, etc.?

I always update everything on the host device via the dashboard. I don't want to miss out on improvements, especially those that improve safety. What this means is that on some occasions, there will be breaking changes: you will need to change a few things in your config **before you can print**. I would suggest that if you absolutely have to get something printed immediately and there's an update, perhaps wait till after the print completes to do the update.

Others will argue that 'if it ain't broke, don't fix it'.

You have to decide what camp you want to be apart off.

#### How-To

##### Update Klipper repo first

\*\* _For those coming from the_ ${\small{\textcolor{WildStrawberry}{\texttt{Before You B.egin}}}}$ _section (installing Klipper for the first time), complete_ **only** _this section_. No need to compile the firmware, etc.

It's always best to update the Klipper repository that lives on the host before compiling. This ensures that your `klipper.bin` will be as 'fresh' as possible.

- In `Fluidd`, you can do this from `Settings` > `Software Updates`.
- In `Mainsail`, you can do this from `Machine` > `Update Manager`.

##### Compile `klipper.bin`

🗒️ The `compilation settings` you see in the image below apply only the the `SV06/Plus` boards, but the steps for compilation apply to any board.

1. `ssh` into the Klipper host (i.e., RPi, OrangePi, etc.).
2. `cd ~/klipper`
3. `make menuconfig`
4. Set things up to look as follows:
   ![make-menuconfig](https://github.com/bassamanator/Sovol-SV06-firmware/assets/61985779/22298d47-2604-4231-ad10-7d6793be7904)
5. `make clean`
   - Clears `~/klipper/out/`
6. `make`
   - Compiles `klipper.bin` and puts it in `~/klipper/out/`
