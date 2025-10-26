# Initial setup

- download the Windows 11 ISO from the official Microsoft website
- create a bootable USB drive (I prefer using Ventoy for this purpose as it allows for easy management of multiple ISOs)
- turn off target machine and insert the bootable USB drive
- disconnect ethernet cable to avoid automatic updates during installation (or annoying activations prompts)
- power on the machine and enter the BIOS/UEFI settings (usually by pressing F2, F12, DEL, or ESC during startup)
- change the boot order to prioritize booting from the USB drive
- save changes and exit BIOS/UEFI settings
- the machine should now boot from the USB drive, presenting the Windows 11 installation screen (or Ventoy menu if using Ventoy - select the Windows 11 ISO from the list)
- follow the on-screen prompts to select language, time, and keyboard preferences
- if you are hit with any activation prompts where you cannot continue (Microsoft is making it harder to skip online activation), select "I don't have a product key" to proceed without entering a key. If you still cannot proceed, press Shift + F10 to open Command Prompt and type "OOBE\BYPASSNRO" then press Enter. This will restart the setup and allow you to bypass the network requirement.
- choose the Windows 11 edition you have a license for (if prompted)
- finish installation and log in for the first time
- reconnect the ethernet cable or set up Wi-Fi to get online
- activate Windows using your product key, digital license or [MAS](https://massgrave.dev/) (only do this if you trust the source and want to test the software before purchasing a license) to gain full functionality
- install all available updates via Settings > Update & Security > Windows Update
- restart the machine if prompted after updates
- if using a laptop, make sure you have sufficient battery charge or keep it plugged in during the setup process as some firmware updates will refuse to install on low battery
- after installing updates check for updates again as some require multiple passes to fully install to latest state
- restart the machine after finishing the first clean setup and updates to ensure all changes take effect
- enable Windows Remote Desktop if you plan to access the machine remotely (Settings > System > Remote Desktop) or set up RustDesk for easier remote setup (recommended unless you enjoy typing on a tiny keyboard and using a trackpad)
- open cmd or PowerShell and check your IP address using `ipconfig` to note it down for remote access later. You may also want to set a static IP address via your router settings to ensure it doesn't change (recommended for pains-free remote access). If you intent to connect to any machine over the internet, do NOT expose RDP directly - use a VPN like WireGuard and connect via LAN IP address instead.
- set up battery plans and screen timeout settings according to your preferences via Settings > System > Power & battery
- make sure to set up a strong password for your user account to enhance security
- install additional drivers based on your component manufacturers' recommendations (optional, Windows usually does a good job at this automatically)

> **NOTE** If you cloned a drive from another machine (using CloneZilla or some other software), Windows 11 has this quirk where it will not be able to boot (0x7b) even if everything was done correctly. You may be tempted to waste your life away by running stuff like chkdsk or changing BIOS settings, but the actual fix is to get into Windows Recovery Environment (WinRE) using a bootable Windows 11 USB drive (or if you manage to get to it on the cloned drive), then navigate to Troubleshoot > Advanced options > Command Prompt and run the following commands:
>
> ```powershell
> # Enable boot into safe mode
> bcdedit /set {default} safeboot minimal
> # Restart the machine
> shutdown /r /t 0
> ```
>
> After the machine restarts, it should boot into Safe Mode successfully. Then open Command Prompt again and run:
>
> ```powershell
> # Disable safe mode boot
> bcdedit /deletevalue {default} safeboot
> # Restart the machine
> shutdown /r /t 0
> ```
>
> After the machine restarts again, it should boot normally without issues. If you are having BitLocker issues, disable it before cloning and re-enable it afterwards.

# First steps

- enable long path support via Registry Editor (regedit):
  - navigate to `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\FileSystem`
  - find the entry named `LongPathsEnabled` and set its value to `1`
  - restart the machine to apply the changes (you can do this later)
- set Terminal as the default terminal application via Settings > Privacy & Security > For developers > Terminal
- we will want to get rid of Windows bloatware, telemetry, and unnecessary services. You can do that by using Chris Titus' [winutil](https://github.com/ChrisTitusTech/winutil) script (or any similar debloating script that you trust sufficiently). Follow the instructions on the GitHub page to run the script and remove unwanted components. When making tweaks it is also recommended to create a system restore point beforehand in case something goes wrong (newer versions do this automatically). Also create an export of the config file so you can reuse it later if needed.
- choose a package manager for easier setup - winget is built-in and works well, but you can also install Chocolatey or Scoop if you prefer those.
- press `ctrl + shift + esc` to open Task Manager, navigate to the Startup tab, and disable any unnecessary startup programs to improve boot times.
- turn on Virus & Threat Protection via Windows Security to ensure your system is protected against malware and other threats. You can also install a third-party antivirus if you prefer, but Windows Defender is quite capable nowadays. Malwarebytes is a solid complementary tool for on-demand scans.
- install your preferred web browser (I recommend Firefox or Brave for better default privacy) - or some other Chromium (ungoogled-chromium) or Firefox (LibreWolf or Waterfox) forks if you prefer even more privacy-focused options. Browsers without a built-in adblocker will benefit greatly by installing the following extensions:
  - uBlock Origin
  - Privacy Badger
  - SponsorBlock
  - Return YouTube Dislike
  - password manager of choice (do not store credentials directly in the browser) - Bitwarden is a great free and open-source option
- useful software

  - 7-Zip (for file archiving / unarchiving)
  - Git (if you plan to do any development work)
  - Monitorian (for easy multi-monitor brightness control)
  - PowerToys (for various productivity enhancements)
  - media player like VLC or MPV
  - image viewer like JPEGView or IrfanView
  - HWInfo (for detailed hardware information and monitoring)
  - CrystalDiskInfo (for monitoring SSD/HDD health)
  - CrystalDiskMark (for benchmarking storage devices)
  - WireGuard (for VPN setup)
  - VS Code or any other code editor if you plan to do development work
  - WireShark (if you need advanced network analysis)
  - Gimp or Krita (for image editing)
  - Da Vinci Resolve (for video editing)
  - WizTree (for disk space analysis)
  - Go (if you plan to do any Go development)
  - Node.js (if you plan to do any JavaScript/TypeScript development)
  - Python (if you plan to do any Python development)

- make sure to set defaults to the programs you prefer via Settings > Apps > Default apps
- if you want gaming support install the following:
  - Nvidia App or AMD Adrenalin Software (depending on your GPU)
  - Steam
  - Epic Games Launcher
  - ...
- for communication apps install:
  - Signal (for privacy-focused messaging)
  - Discord (more gaming-oriented)
  - Telegram
  - ...
- torrent client for downloading Linux ISOs and other large files:
  - qBittorrent (lightweight and ad-free, similar to uTorrent)
  - Transmission (simple and minimalistic)
- enable Hyper-V if you plan to use virtual machines (Settings > Apps > Optional Features > More Windows features > check Hyper-V)
