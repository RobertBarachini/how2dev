# Prerequisites

Before you begin with setting up the VM you'll need to get the ISO of the operating system you want to install. I'll be using Kubuntu 24.04 LTS as it is the latest LTS release of Kubuntu at current time. You can choose another operating system if you want, but you may need to adjust certain commands and steps to fit accordingly.

Visit the [Kubuntu website](https://kubuntu.org/getkubuntu/) and download the ISO for the latest LTS release. You can choose to download the torrent or the direct download. I recommend the torrent as it is faster and more reliable. Torrent link: [Kubuntu 24.04 LTS (64-bit)](http://cdimage.ubuntu.com/kubuntu/releases/noble/release/kubuntu-24.04-desktop-amd64.iso.torrent). Once the download finishes it is advisable to check the checksum of the file. Checksum can be found here: [Kubuntu Alternative Downloads](https://kubuntu.org/alternative-downloads/).

# Setting up the VM

## Introduction

Considering the majority of the readers are using Windows, we'll be using Hyper-V to set up the VM. Hyper-V is a Type 1 hypervisor which is built into Windows 10 Pro, Enterprise, and Education. If you are using Windows 10 Home, you can use VirtualBox or VMware Workstation Player. If this is your first time using a hypervisor you may need to enable it first. See [Install Hyper-V on Windows 10](https://learn.microsoft.com/en-us/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v). If you are using a Unix-based system you can skip to `TODO: add link.`

Once you have installed and enabled Hyper-V, you can start Hyper-V Manager by searching for it in the Start menu. You can also run `virtmgmt.msc` in the Run dialog (Win + R). I also suggest you pin it to the taskbar for easy access as you'll be using it quite often.

## Create a new VM

1. Open Hyper-V Manager.
2. Click on `Action` in the menu bar.
3. Click on `New` and then `Virtual Machine`.
4. Click `Next` on the `Before you begin` screen.
5. Enter a name for the VM (the name should be something descriptive like `Kubuntu 24.04 LTS Develop` or `Kubuntu Dev`) and click `Next`.
6. Choose the generation for the VM - select `Generation 2` and click `Next`. Most modern operating systems support Generation 2 VMs.
7. Assign memory to the VM - I recommend at least 8GB (8192MB) of memory. You can assign more if you have enough memory to spare. Click `Next`. You can keep the default setting for `Use Dynamic Memory for this virtual machine`, however I prefer to disable it as it can cause performance issues.
8. Configure networking - select `Default Switch` and click `Next`. We will change this to a custom switch later.
9. Connect a virtual hard disk - I using the `Create a virtual hard disk` option and click `Next`. You can choose the location and size of the virtual hard disk. The default 127GB is fine, but you can increase it if you have enough space. I recommend setting it at 200GB. It can be increased later if needed.
10. Installation options - select `Install an operating system from a bootable image file` and click `Browse`. Select the ISO file you downloaded earlier and click `Next`.
11. Summary - review the settings and click `Finish`.

## Initial configuration of the VM

1. Right-click on the VM you just created and click `Settings`.
2. Under `Security` disable `Secure Boot`.
3. Under `Processor` increase the number of processors to at least 2. I recommend setting the number of processors to the number of logical processors on your host machine minus two (e.g. if you have 12 logical processors or cores, set it to 10).
4. Under `Integration Services` enable `Guest Services`.
5. Click `Apply` and then `OK`.

We are now ready to boot up the VM for the very first time and install the operating system. When you are ready to proceed, right-click on the VM and click `Start`. Right-click on the VM again and click `Connect` to open the VM console.

Continue with [Installing Kubuntu 24.04 LTS](installing-kubuntu.md) and return here once you have completed the installation.

## Configure the VM

Once you have selected to restart the system after the installation, you may need to remove the ISO from the VM. Right-click on the VM and click `Settings`. Under `Media` select the ISO file and click `Remove`. Click `Apply` and then `OK`. Alternatively you can do so from the VM console by clicking on `Media` in the menu bar and selecting `Eject disk`.

If you decided to use system encryption during the installation, you will be prompted to enter the encryption password. Enter the password and press `Enter`. You will be presented with the login screen. Enter the password you set during the installation and press `Enter`.

Once you have logged in, I suggest you open this guide in a browser on the VM as it will make it easier to copy and paste commands as the enhanced session doesn't work out of the box.

Open a terminal by pressing `Ctrl + Alt + T` and run the following commands:

```shell
sudo apt-get update -y && sudo apt-get upgrade -y
sudo apt install curl net-tools -y
```

If you plan on using the VM GUI a lot you can also pin the terminal to the taskbar for easy access. Right-click on the terminal icon in the taskbar and select `Pin to Task Manager`.

### Hyper-V Linux Enhanced Session

Q: Why?

A: Enhanced session will make it possible to use the clipboard and drag and drop functionality between the host and the VM. It will also make it possible to connect to the VM using a similar protocol to RDP (Remote Desktop Protocol) which will make it possible to use the VM in a windowed mode and with multiple monitors.

To enable the enhanced session, follow the steps:

1. Run the following commands in the terminal inside the VM to install the required packages:

```shell
cd ~/Downloads
wget https://raw.githubusercontent.com/Hinara/linux-vm-tools/ubuntu20-04/ubuntu/20.04/install.sh
chmod +x install.sh
sudo ./install.sh
rm install.sh

# NOTE: Microsoft has stopped maintaining their version of `linux-vm-tools` so we are using a newer forked version
# Their version can be found here: https://github.com/microsoft/linux-vm-tools or https://raw.githubusercontent.com/Microsoft/linux-vm-tools/master/ubuntu/18.04/install.sh
```

> NOTE: If you want to use a different keyboard layout than the default US layout on the xrdp login screen, you can run the following commands to generate a new keyboard layout file and set it as the default. You can find the list of available keymap names [here](https://sourceforge.net/p/rdesktop/code/1704/tree/rdesktop/trunk/doc/keymap-names.txt). Note that you cannot generate a keymap if you have logged on to the VM using the enhanced session. You will need to temporarily disable xrdp service and enhanced session and then generate the keymap and re-enable the services.
> Since I'm Slovenian, I'll be using the Slovenian keymap as an example.

```shell
sudo xrdp-genkeymap /etc/xrdp/km-00000424.ini
sudo tee -a /etc/xrdp/xrdp_keyboard.ini > /dev/null <<EOT

# Added layout
rdp_layout_sl=0x00000424
rdp_layout_sl=sl

EOT
sudo systemctl restart xrdp
sudo shutdown -h now
```

2. When the VM is in the powered off state, open the PowerShell as an administrator and run the following command:

```powershell
Set-VM -VMName "VM_NAME" -EnhancedSessionTransportType HvSocket
# NOTE: VM_NAME is the name of the VM you set during the creation - e.g. "Kubuntu 24.04 LTS Develop"
```

3. Start the VM and connect to it. You should see a dialog box asking you to configure the enhanced session. You can configure the settings as you see fit. Once you have configured the settings, click `Connect`.

With some luck you should now enjoy the full power of the enhanced session. Test the clipboard and other functionalities to your heart's content.

> NOTE: If you are having issues try the following:
>
> Try editing `/etc/xrdp/xrdp.ini`:
>
> ```ini
> port=vsock://-1:3389
> use_vsock=false
> ```
>
> If it still doesn't work try the following:
>
> ```shell
> sudo apt-get update -y && sudo apt-get upgrade -y
> sudo apt-get purge xrdp
> sudo apt-get install xrdp
> sudo systemctl restart xrdp
> sudo reboot now
> ```

### Set up a static IP address

To set up a persistent SSH connection we will need a static IP address. To enable this we will first need to make some adjustments on our host system and create a new virtual switch. If you have already created a virtual switch you can skip to the next section. To create a new virtual switch inspect [Create a new virtual switch](virtual-switch.md).

Once you have created a new virtual switch you will need to make some changes to the network settings on the VM. Inside the Hyper-V Manager right-click on the VM and click `Settings`. Under `Network Adapter` select the new virtual switch you created and click `Apply` and then `OK`. This will disconnect the VM from the default switch. To set up a static IP connection on the VM, follow the steps:

1. Open settings by clicking the super key (Windows key) and typing `Settings` and pressing `Enter`.
2. Search `Connections` and click on `Connections`.
3. Click on the `+` icon to add a new `Wired Ethernet` connection.
4. Click on the `IPv4` tab and set the `Method` to `Manual`.
5. Set `DNS Servers` to `192.168.0.1,8.8.8.8,8.8.4.4`.
6. Click the `+` icon to add a new address.
7. Set `Addresses` to `192.168.0.24` (we'll use this address for the VM in this guide).
8. Set `Netmask` to `255.255.255.0`.
9. Set `Gateway` to `192.168.0.1`.
10. Click `Apply`.
11. Right click on the newly created connection and click `Connect`.

Verify that the IP address is set correctly by running the following command in the terminal:

```shell
ip a
# or alternatively
ifconfig -a
```

You should now have a persistent connection to the VM. You can test the connection by pinging the VM from the host machine. Open a PowerShell window and run the following command:

```powershell
ping 192.168.0.24
```

You should see a response from the VM. If you don't see a response, double-check the settings and make sure the VM is running.

Additionally check that your VM can access the internet by pinging a public IP address:

```shell
ping www.google.com -c 4
```

Sometimes restarting the VM and reconnecting the network adapter can help if you're having issues.

### Set up SSH

To set up SSH on the VM, run the following commands:

```shell
sudo apt install openssh-server -y
sudo systemctl status ssh
sudo ufw allow ssh
```

We will now perform a proper setup.

To improve the default security settings, we will make some changes to the SSH configuration file. Open the SSH configuration file in a text editor:

```sh
sudo nano /etc/ssh/sshd_config
```

and add the following lines to the file:

```
Port 2222
PasswordAuthentication no
PermitRootLogin no
PermitEmptyPasswords no
```

or by running the following command:

```sh
sudo tee -a /etc/ssh/sshd_config > /dev/null <<EOT

# Custom settings
Port 2222
PasswordAuthentication no
PermitRootLogin no
PermitEmptyPasswords no

EOT
```

Then restart the SSH service:

```shell
sudo systemctl restart ssh
# or depending on your system
# sudo systemctl reload sshd
# or
# sudo service sshd restart
```

1. On the host machine open a PowerShell window and run the following commands to generate a new SSH key:

> NOTE: You may need to install Git for Windows to get the `ssh-keygen` command. You can download it from [Git for Windows](https://git-scm.com/download/win).
>
> NOTE: If the command doesn't work you may need to enable the SSH feature on Windows 10 first. You can install OpenSSH with PowerShell:
>
> ```powershell
> Add-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0
> # NOTE: check the version of the OpenSSH client and adjust the command accordingly
> ```

```powershell
cd C:\Users\$env:UserName\.ssh
ssh-keygen -t ed25519 -f VM-Kubuntu-2404-Dev
# NOTE: it is a good practice to use a passphrase otherwise anyone with access to your key can access the VM
```

Generated private and public keys will be located in `C:\Users\%username%\.ssh` on Windows and in `/etc/ssh` if generated on Linux.

2. You will need to copy the newly generated public key `VM-Kubuntu-2404-Dev.pub` to `~/.ssh` in your guest (VM). If the folder doesn't already exist, create it (`mkdir ~/.ssh`). Copying keys between Unix systems is easy with the `ssh-copy-id` command, however, it doesn't work on Windows. You can test out copying the file with `ctrl + c` and `ctrl + v` if you properly set up the enhanced session.

3. Once you have copied the public key to the VM, you can add it to the authorized keys by running the following command:

```shell
cat ~/.ssh/VM-Kubuntu-2404-Dev.pub >> ~/.ssh/authorized_keys
```

4. To make it easier to connect to the VM you can add the following to your `C:\Users\%username%\config` file on the host machine (create a new file if it doesn't exist):

```
Host VM-Kubuntu-2404-Dev
	HostName 192.168.0.24
	User main
  IdentityFile ~/.ssh/VM-Kubuntu-2404-Dev
  IdentitiesOnly yes
	Port 2222
```

> NOTE: Make sure to set the correct `User`, `IdentityFile`, and `HostName` (IP) values if you chose other than default in this guide.

5. You can now connect to the VM by running the following command in PowerShell:

```powershell
ssh VM-Kubuntu-2404-Dev
```

When asked if you want to continue connecting, type `yes` and press `Enter`. You should now be connected to the VM from host's terminal.

If it doesn't work try restarting the SSH service or the VM.

> NOTE: When starting from a powered off state, you may need to log in via the GUI first to start the SSH service. You can do so by connecting to the VM using the enhanced session.

> NOTE: There is a long-standing issue where any subsequent enhanced sessions will reset the guest system's keyboard layout. You can fix this by running `setxkbmap si` in the terminal. Replace the "si" with your desired layout. This can be put into various files to run automatically on login, however none of the workarounds work consistently.

Temporary workaround (example for Slovenian keyboard layout):

```sh
sudo tee -a /etc/xrdp/reconnectwm.sh > /dev/null <<EOT

# Reset keyboard layout on xrdp log on
sleep 2
. ~/.resetkb

EOT
sudo tee -a /etc/xrdp/startwm.sh > /dev/null <<EOT

# Reset keyboard layout on xrdp log on
sleep 2
. ~/.resetkb

EOT
sudo tee -a /etc/xrdp/startubuntu.sh > /dev/null <<EOT

# Reset keyboard layout on xrdp log on
sleep 2
. ~/.resetkb

EOT
sudo tee -a ~/.resetkb > /dev/null <<EOT

# Set keyboard layout to Slovenian
setxkbmap si

EOT
```

If you wish to remove SSH, you can run

```sh
sudo apt-get remove ssh-server --purge
```

This setup will allow us to streamline development - especially when paired with VS Code's remote development extension.

You're now ready to start rocking! 🚀

# Next steps

Here are some additional steps which will help you level up further.

## OS preferences

Check out [OS preferences](kubuntu-preferences.md) for some additional settings and tweaks you can make to your OS.

## Utilities, tools, and software

Check out the [Utilities, tools, and software](utils.md) guide for a list of tools and software you may find useful.

## Terminal

Enhance your terminal experience by checking out the [Terminal](terminal.md) guide.

## VS Code

Set up [VS Code](vscode.md) for a better development experience.
