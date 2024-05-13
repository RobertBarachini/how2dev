# Installation

When the system boots up and you are presented with the GRUB menu, select the first option `Try or Install Kubuntu` and press `Enter`. If the system doesn't boot try resetting the VM and try again. Sometimes you just need to chill out a bit and wait - but don't wait for eternity. If it still fails (it frequently displays an underscore on a black screen), you may select the option `Kubuntu (safe graphics)` and press `Enter`.

Once presented with the installer, select `Install Kubuntu`.

1. Select your language and click `Next`.
2. The settings on the `Location` screen should be preset correctly, however you can set the region, zone, system language, and locale if needed. Click `Next`.
3. Select your keyboard layout and click `Next`.
4. Under `Customize` keep the default settings and click `Next`. You can tick `Download and install update following installation` as well.
5. Under `Partitions` select `Erase disk`. The dropdowns should be preset correctly (`Swap to file` and `ext4`). You can choose to encrypt the system as well but it's not needed - especially if you are using encrypted partitions or drives on your host system already. Click `Next`.
6. Under `Users` enter your name, login name, name of the computer, and password. Click `Next`. We'll be using `main` as a generic name for the user and computer.
7. Check the settings on the `Summary` screen and click `Install` and then `Install Now`.
8. Wait for the installer to finish. You can track the progress at the bottom of the screen. The process takes a couple of minutes on an NVMe SSD.
9. Once the process concludes, select `Restart Now` and click `Done`.

If you experience any issues after the installation like a black screen, try entering the recovery mode and select `dpkg` to fix broken packages and `grub` to update the bootloader. If you are still experiencing issues, you may need to reinstall the system. There are currently known issues with `Kubuntu 24.04 LTS` independent of chipset and graphics card. You can enter the recovery mode by pressing `Shift` (more likely) or `Esc` during boot and selecting `Advanced options for Kubuntu` and then `Recovery mode`. Booting into recovery mode may take a couple of attempts as it is a bit finicky. If the computer doesn't boot after repeatedly pressing `Shift`, try stopping the VM and starting it again and trying to hit the key when the logo appears or when four dots are displayed.
