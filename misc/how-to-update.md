\*\* _Flashing the firmware is not required for any method_

## Should You Update?

I treat this config like software, and like any piece of modern software, it will require periodic updates to fix issues, add features, improve functionality, etc. So, if you want to have the latest and greatest, I recommend you periodically pull the changes from this repo.

### What Changes Should You Be Concerned With

The only changes you need to be concerned with relate to the `.cfg` and `.conf` files; all changes to `.md` files (these are markdown files, the text that you see when you visit this repo) can be ignored.

### Git Users

If you do a `git diff origin/master --name-only` and it shows you that a `.cfg` file has changed, you might want to pull in those changes (but you don't have to).

### Non-Git Users

Unfortunately, there's no simple way for you to ascertain whether there have been changes. I simply recommend that you update your config via the instructions below from time to time.

## How to Update

There are three methods to accomplish this:

1. The correct method, using `git`.
2. The mostly acceptable method, without using `git`.
3. The start-from-scratch method. _Recommended for non-git users and those who haven't made many changes to their configuration._

### The `git` method.

⚠️ This method is only intended for those who already know how to use `git`.

📝 In this example, I'm using the `master` branch which applies to SV06 users only. Be sure to use the appropriate branch for your printer.

📝 The assumption is that you didn't change branches after the initial setup, so you are in `master`, and there are many untracked changes.

1. `ssh` into your Klipper host.
2. `cd ~/printer_data/config`
3. `git checkout -b my-settings`
4. `git add .`
5. `git commit -m "Saving my settings."`
6. `git checkout master`
7. `git pull`
8. `git checkout my-settings`
9. `git merge master --no-ff --no-commit`
10. Deal with comparing and pulling in incoming changes from `master` using your favourite code editor. You can also do this from the command line via `git commit --interactive`, however, only advanced `git` users should attempt this, though it is easy enough.
11. Add all welcomed changes to the staging area, and discard all other changes.
12. `git commit -m "Update with upstream."`

You should push your branch to your own fork of this repo.

For any future updates, you can run through the same process again, however, you cannot re-create the `my-settings` branch as you did in `step 3`, because it already exists. Simply omit the `-b` flag in `step 3` next time you update.

### The mostly acceptable method.

This method has shortcomings, because it relies on the user's memory, and requires more manual edits. Perfectly functional method, however.

1. Read all the documentation.
2. Backup your current configuration, essentially everything inside `~/printer_data/config`.
3. Repeat steps in [Download OSS Klipper Configuration](https://github.com/bassamanator/Sovol-SV06-firmware#download-oss-klipper-configuration), and [Adjust Configuration with MCU Path](https://github.com/bassamanator/Sovol-SV06-firmware#adjust-configuration-with-mcu-path).
4. Copy everything from `#*# <---------------------- SAVE_CONFIG ---------------------->` onward and paste into new `printer.cfg`, inclusive.
5. Copy any other changes you might have made into the new configuration. Maybe you had adjusted the size of your printer (`position_max`), or other such changes.

### The start-from-scratch method.

1. Delete the folder `~/printer_data/config`.
2. Recreate the folder `~/printer_data/config` via `mkdir ~/printer_data/config`.
3. Start the Klipper installation process starting from `Download OSS Klipper Configuration` in the appropriate branch.

## Linux Tips

- In linux, you can delete files via `rm fileName` and directories via `rmdir directoryName`.
- In linux, you can list files and folders via `ls -lah`.

You are now up-to-date with this repo, and have added your personal settings on top.
