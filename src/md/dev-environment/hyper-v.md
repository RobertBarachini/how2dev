# Hyper-V

Has your hand been forced to use Windows as the host OS by your organization, Big Microsoft, or some other corpo ecosystem? Still need to run Linux or some other OS? Well, you're in luck, as Hyper-V, Microsoft's hypervisor for Windows (Pro, and the likes), is a great choice for creating VMs and a cozy development environment.

# Management

## Compacting, shrinking, and archival

When you create a dynamically-expanding VHDX (virtual hard disk) in Hyper-V and then install a Linux-based (UNIX ☝️🤓) OS, the disk will grow as needed. As such, its associated file will grow as well. Let's say we set the disk to 127GB (the default) and then install Ubuntu, do some other stuff which takes up space, and then delete some of the files. Although we had deleted the files, the VHDX files (after applying or merging all snapshots/changes) will still be the same or even larger than before. This is normal. Hyper-V also offers tools, like `Optimize-VHD`, to compact the VHDX file. Compacting works well for Windows-ish filesystems, but it doesn't always work as expected on other filesystems, like ext4, which is common for Linux. Additionally some operations like compacting or shrinking the VHDX file are still so badly implemented that the management software doesn't really know what to do with ext4 partitions and it at best leaves them be (clearing zero reclaimable space, even if zeroing out the unused space beforehand) or at worst corrupt the partition table (GPT) and make the disk unbootable (usually drops you into busybox). After years of finding a solution I have come up with a process that works just well enough(TM). It still sucks, as the built-in tools should be able to handle this by now, but alas...

> WARNING: Here be dragons. Make appropriate backups (of the "shut down" VHDX file).

1. Delete unnecessary files and folders inside the VM (client OS).
2. Shut down the VM and delete any snapshots (wait for the merge to complete) in `Hyper-V Manager`. It is recommended you back up the VHDX file before proceeding.
3. Attach the VHDX to another Linux VM or live environment, start the secondary system, and launch `GParted` (as root). Other partitioning tools, like `parted` work as well, but `GParted` is more user-friendly.
4. Select the disk of the primary VM that you want to edit (will probably be listed as `/dev/sdb`) and select the ext4 system partition (will probably be `/dev/sdb2` - it is the largest partition and the only one with a filesystem on it).
5. Select `Check` by right-clicking on the partition or by selecting it and then selecting `Partition` -> `Check`.
6. Apply the changes and wait for the check to complete. Optionally inspect the output for any errors.
7. Select `Resize/Move` by right-clicking on the partition and resize it to the `available space + 1GB` (or whatever extra space you need).
8. Apply the changes and wait for the resize operation to conclude. Optionally run the check again.
9. Shut down the VM.
10. Open original VM's settings, select the disk, and then select `Edit`.
11. Under `Choose Action` select `Shrink` and wait for the operation to complete.
12. Under `Choose Action` select `Compact` and wait for the compact operation to complete. The VHDX file will finally be smaller than the original but it will also have a corrupt GPT partition table (shrink ftw).
13. Attach the VHDX (probably already attached from before) to another Linux VM and open a terminal.
14. Run `sudo gdisk /dev/sdb` (or whatever the disk is) and select `v` to verify the disk. If it says the GPT is corrupt, select `x` and then `e` to fix it. To write the changes, select `w` and then confirm. After the process is complete, you can exit `gdisk`.
15. Open `GParted` again and select the correct disk and partition. Select `Check` and apply the changes. Wait for the check to complete. If GParted moans and gives a warning about the partition table, you are probably SOL and should restore the backup you made and retry the process in some other magical way. If the check completes without any errors, you're golden.
16. Shut down the VM and detach the VHDX.
17. Start the original VM and verify that it boots and works correctly.

Now you have a smaller VHDX file with a working GPT partition table. Expanding the disk is usually way easier.

If you have a better solution, or work at Microsoft, please let me know.

# Recommendations

- Use a dynamically-expanding VHDX file for the VM disk.
- Disable `Dynamic Memory` in the VM settings. Set some reasonable amount of memory, like 8192MB (8GB). Dynamic memory (with a set limit) personally caused me many headaches - especially with Linux VMs. Windows VMs seem to handle it better.
- Set a reasonable number of virtual processors (vCPUs) - I usually set it as the number of actual cores in the host machine minus two.
- Use a custom switch to create a private network between the host and all the VMs. This makes it easier to manage connections, set up static IP, and manage configs, like SSH. For more info check out the [virtual switch](virtual-switch.md) guide.
- Enable `Enhanced Session` in the VM settings. This will allow you to use the clipboard, share files, and use other features between the host and the VM. Some OSes may require additional stuff to be installed - for more info check out the [VM setup](vm-setup.md) guide.
