# Introduction

A virtual switch is a software-based network switch that allows virtual machines to communicate with each other and the host machine. We will create a custom internal virtual switch that will allow us to communicate between the host and the guest machine. This is especially useful if you want to set up a persistent SSH connection between the host and the guest machine or between multiple guest machines.

# Create a new virtual switch

> NOTE: This guide is adapted from https://superuser.com/questions/1541599/how-to-configure-static-ip-address-for-a-hyper-v-vm-ubuntu-19-10-quick-create

1. Open a PowerShell window as an administrator.
2. Run the following command to create a new internal virtual switch:

```powershell
New-VMSwitch -SwitchName "SwitchDevelop" -SwitchType Internal
```

3. Run the following command and note the `ifIndex` of the new switch (name is in the first column):

```powershell
Get-NetAdapter
```

4. Run the following command to set a static IP address for the new switch (replace `ifIndex` with the value from the previous command):

```powershell
New-NetIPAddress -IPAddress 192.168.0.1 -PrefixLength 24 -InterfaceIndex <INDEX>
```

5. Run the following command to create a NAT network:

```powershell
New-NetNat -Name MyNATnetwork -InternalIPInterfaceAddressPrefix 192.168.0.0/24
```

> NOTE: If you get an error you may need to remove the previous NAT first.
>
> ```powershell
> Remove-NetNat -Name MyNATnetwork
> ```

> NOTE: If you want to remove the virtual switch you can run the following command:
>
> ```powershell
> Remove-VMSwitch -Name "SwitchDevelop"
> ```
