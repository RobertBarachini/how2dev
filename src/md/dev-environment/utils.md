# Introduction

This guide assumes you have a working version of a Linux distribution installed on your machine. If you don't, check out the [VM Setup](vm-setup.md) guide to get started.

Before running any commands, make sure you have the latest package list and package upgrades by running:

```sh
sudo apt-get update -y && sudo apt-get upgrade -y
```

# Utilities and tools

Git and nano are most likely already installed on your system. If not, you can install them by running the following commands:

```sh
sudo apt-get install git nano -y
```

Similarly, here are some additional useful tools that you might want to install:

```sh
# curl, wget, aria2c
sudo apt-get install curl wget aria2 -y

# unzip, zip, 7zip
sudo apt-get install unzip zip p7zip-full -y

# tree
sudo apt-get install tree -y

# htop
sudo apt-get install htop -y

# ffmpeg
sudo apt-get install ffmpeg -y
# test it:
ffmpeg -version
```

# Git

Check out the [Git](git.md) guide for setting up Git and how to use it.

# Python

Check out the [Python](python.md) guide for setting up Python and how to use it.

# Docker

To check Docker-specific programs, check out the [Docker](docker.md) guide.

```

```
