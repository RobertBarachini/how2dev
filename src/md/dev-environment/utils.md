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

To set up your Git configuration, run the following commands:

```sh
git config --global user.name "Firstname Lastname"
git config --global user.email "username@domain.com"
# Verify the configuration
git config --list
```

When working with remotes, like GitHub, you might want to use SSH keys instead of password authentication or personal access tokens. To generate an SSH key, run the following command:

```sh
cd ~/.ssh
ssh-keygen -t ed25519 -f github-dev
# you can use a passphrase for extra security
```

After generating the SSH key, copy the public key.

```sh
# Print the public key
cat github-dev.pub
```

Add the public key to your GitHub account by going to `Settings > SSH and GPG keys > New SSH key` (at [this](https://github.com/settings/keys) URL). Give it a fitting title, paste the public key, and click `Add SSH key`.

To use the SSH key, you can either add it to the SSH agent or create a `~/.ssh/config` file (with `touch ~/.ssh/config`) and add the following:

```sh
Host github.com
	HostName github.com
	User git
	IdentityFile ~/.ssh/github-dev
	IdentitiesOnly yes
```

To test the SSH key, run the following command:

```sh
ssh -T git@github.com
```

If you see a message like `Hi username! You've successfully authenticated, but GitHub does not provide shell access.`, you're good to go.

If you plan on going the `ssh-agent` route, you can add the key to the agent by running:

```sh
ssh-add ~/.ssh/github-dev
```

If `ssh-agent` is not running, you can start it by

```sh
eval "$(ssh-agent -s)"
```

To make sure the agent is running and the appropriate key is added, run:

```sh
ssh-add -l
```

To automatically start the `ssh-agent` and add the key, you can add the following to your `~/.bashrc` or `~/.bash_profile`:

```sh
# Start the ssh-agent
if [ -z "$SSH_AUTH_SOCK" ] ; then
	eval `ssh-agent -s`
	# Add the key
	ssh-add ~/.ssh/github-dev
fi
```

# Python

Check out the [Python](python.md) guide for setting up Python and how to use it.

# Docker

To check Docker-specific programs, check out the [Docker](docker.md) guide.

```

```
