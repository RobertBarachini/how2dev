# Mamba

I prefer using `Mamba` (a faster and more efficient version of `Conda`) for managing Python environments. To install `Mamba`, run the commands under setup.

## Setup

```sh
# Download the Mamba installer (Mambaforge) for your specific system and architecture
wget "https://github.com/conda-forge/miniforge/releases/latest/download/Mambaforge-$(uname)-$(uname -m).sh"
# Install Mambaforge
chmod +x Mambaforge-$(uname)-$(uname -m).sh
bash Mambaforge-$(uname)-$(uname -m).sh
# During installation keep pressing enter to accept the license agreement
# Then type 'yes' to accept the default installation location
# Press 'ENTER' to confirm the location
# Type 'yes' to set up the shell initialization
# Remove the installer
rm Mambaforge-$(uname)-$(uname -m).sh
```

## Common commands

```sh
# List all environments
mamba env list
# Remove the environment
mamba env remove -n <environment_name>
# Install a package in the specific environment
mamba install -n <environment_name> <package>
# Remove a package from the specific environment
mamba remove -n <environment_name> <package>
# List installed packages in select environment
mamba list -n <environment_name>
```

## Example environment

```sh
# Create a new environment
mamba create -n sample-env python=3.12.3
# Activate the environment
conda activate sample-env
# Install some packages
mamba install numpy pandas matplotlib
# Deactivate the environment
conda deactivate
# Remove the environment
mamba env remove -n sample-env
```

## Example project template

You can check out my Python project template [here](https://github.com/RobertBarachini/python-template)
