# Introduction

Node.js or Node is a JavaScript runtime based on Chrome's V8 JS engine. It's commonly used for creating backends for web apps.

# Setup

## nvm

Node Version Manager (nvm) is a tool that allows you to manage multiple versions of Node.js on a single machine. To install nvm, run the following command:

```sh
# Ensure your system is up to date
sudo apt update -y
# Install nvm
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash
```

Test installation:

```sh
# Source the nvm script
source ~/.bashrc
# Check the nvm version
nvm -v
```

Some random commands:

```sh
# List all available Node.js versions
nvm ls-remote
# List locally installed Node.js versions
nvm ls
# Install a specific Node.js version
nvm install <version>
# Use a specific Node.js version
nvm use <version>
# Upgrade nvm
nvm upgrade
```

## Node.js

It is advisable to install the latest LTS version of Node.js. To install the latest LTS version, run the following command:

```sh
nvm install --lts
# Alternatively you can run the following command to install the latest version
nvm install node
# Verify the installation
node -v
```

Additionally, you should match your project's Node.js version to the one you have installed. Make sure you edit the appropriate file (like `Dockefile`).

## pnpm

pnpm is an alternative to `npm` (Node Package Manager). It is more efficient.

```sh
# Install pnpm
npm install -g pnpm
```

# Templates

I've created a couple of templates for Node.js projects. You can check them out here:

- [Node.js API template](https://github.com/RobertBarachini/nodejs-template)
- [npm package template](https://github.com/RobertBarachini/npm-package-template)

# Development

TODO
