# Introduction

TODO

# Setup

```sh
mkdir -p ~/Downloads && cd ~/Downloads
wget https://go.dev/dl/go1.23.4.linux-amd64.tar.gz
sudo rm -rf /usr/local/go && sudo tar -C /usr/local -xzf go1.23.4.linux-amd64.tar.gz
export PATH=$PATH:/usr/local/go/bin >> ~/.bashrc
source ~/.bashrc
go version
```