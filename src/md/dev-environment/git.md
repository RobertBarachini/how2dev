# Installation

Most distros come with Git pre-installed. To check if Git is installed, run the following command:

```sh
git --version
```

If Git is not installed, you can install it by running:

```sh
sudo apt-get install git -y
```

# Setting up Git

To set up a global Git configuration for your username, run the following commands:

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

# Git commands

TODO

# Workflows

TODO

# Hooks

TODO
