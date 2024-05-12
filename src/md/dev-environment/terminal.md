# Introduction

Pimping your terminal can help you be more productive as well as reduce oopsie daisies or bruh moments by providing additional information and visual cues like git status, Python virtual environment, and more.

## Font

### Cascadia Code (Nerd Fonts)

It is a really neat mono font which has a lot of glyphs, icons, and ligatures. Its base variant is also the default font for Windows Terminal.

```sh
cd ~/Downloads
wget https://github.com/ryanoasis/nerd-fonts/releases/download/v3.2.1/CascadiaCode.tar.xz
mkdir CascadiaCode
tar -xf CascadiaCode.tar.xz -C CascadiaCode
cd CascadiaCode
sudo mkdir -p /usr/share/fonts/truetype/CascadiaCode/
sudo cp *.ttf /usr/share/fonts/truetype/CascadiaCode/
cd ..
rm -rf CascadiaCode
sudo fc-cache -fv
```

Check if it works in Konsole by going to `System Settings -> Fonts -> Fixed width` and change to `CaskaydiaCove Nerd Font Mono 12pt`. Test by opening a new instance of Konsole and type `=>` and it should be replaced by a right arrow.

Alternative font install:
Open `CascadiaCode/ttf` directory in file explorer, select all extracted `.ttf` files, right click and click `Install`.

## Oh My Posh

Oh My Posh is a prompt theme engine. You can set it up on pretty much any shell you use.

```sh
# Install
sudo wget https://github.com/JanDeDobbeleer/oh-my-posh/releases/latest/download/posh-linux-amd64 -O /usr/local/bin/oh-my-posh
sudo chmod +x /usr/local/bin/oh-my-posh

# NOTE: If you have been using an older version of Nerd Fonts and have now installed >= 3.0.0, you may need to fix your Oh My Posh theme by updating it to the latest version and running the following command (they moved codepoints and removed some glyphs):
# oh-my-posh config migrate glyphs --write

# Download themes
mkdir -p ~/.poshthemes
wget https://github.com/JanDeDobbeleer/oh-my-posh/releases/latest/download/themes.zip -O ~/.poshthemes/themes.zip
unzip ~/.poshthemes/themes.zip -d ~/.poshthemes
chmod u+rw ~/.poshthemes/*.json
rm ~/.poshthemes/themes.zip
```

You can find themes in `~/.poshthemes/` directory. You can set a theme by running `eval "$(oh-my-posh --init --shell bash --config ~/.poshthemes/<themeName.omp.json>)"` in your shell.

To make it permanent, add the above command to your shell's config file. For example, for bash, add it to `~/.bashrc`.

Below example uses my custom theme `mytheme.omp.json`. You can find the theme in [mytheme.omp.json](configs/mytheme.omp.json).

```sh
# 1. Copy theme to ~/.poshthemes/
#
# 2. Add init command to shell config file (.bashrc in this case) by running the following command:
tee -a ~/.bashrc <<EOF

# Oh My Posh
eval "$(oh-my-posh --init --shell bash --config ~/.poshthemes/mytheme.omp.json)"

EOF
# 3. Reload shell
source ~/.bashrc
```
