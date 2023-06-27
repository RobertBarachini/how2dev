# Introduction

A good environment can be a make or break for a project. It can be the difference between a fun and productive day and one filled with frustrations.

First things first - develop in an isolated and reproducible environment. This is especially important if using Windows. While it may be suitable for certain project, it is highly recommended to use a virtual machine (VM). If you're using a Mac, you can run the code natively (for the most part), however it is also highly recommended to use a VM for various reasons described below.

Benefits of using a VM:

- Isolated environment - no need to worry about messing up your host machine
- Reproducible environment - no need to worry about different versions of tools, etc.
- Easy to backup and restore on a new machine - you can export a VM hard drive or the whole configuration and migrate it to a new machine or periodically backup the VM
- State management (snapshots) - you can take snapshots of the VM at any point in time and revert to them if needed (e.g. before installing a new tool or running rm -rf on your root directory). You can also save current state of a running VM (which "dumps" the memory to disk) and restore it later (e.g. if you need to reboot your host machine) at that exact point in time.
- Host agnostic - you can run the same VM on any host machine (Windows, Mac, Linux) and it will work the same way (some specifics may vary)
- Similar to a production environment - you can run the same OS and tools in production as you do in development (with additions of a desktop environment and other tools you may need for development)

My personal preference is running Kubuntu (a Linux distribution which is built on top of Ubuntu, a very popular distribution, commonly used in server environments) in Hyper-V as it has all the features you need for a good overall experience, and with some tweaks, you can vastly improve the experience over using something like WSL2 or VirtualBox (better Enhanced Session, integration services, and performance). If you're using Windows (10 / 11) (Pro / Enterprise / Education) you have the option to enable just enable Hyper-V without having to install third party software. I'll be sharing my setup and configuration in this guide.

Some basic terminology:

- `Host machine` - the machine you're running the VM on
- `Guest machine` - the machine you're running inside the VM
- `VM` - virtual machine
- `Hypervisor` - software that runs the VM (Hyper-V, VirtualBox, VMWare, etc.)
  > NOTE: There are multiple different types of hypervisors, but we won't be going into that here

# Setup

## VM

The following instructions detail how to setup a VM with Kubuntu 20.04.2 LTS (Long Term Support) using Hyper-V: [VM Setup](vm-setup.md)

## Tools and software
