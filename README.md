# Carv Node Installation Guide

This guide will walk you through downloading and installing a specific version of the Go compiler environment on Ubuntu 20.04, and compiling the Carv node.

## Prerequisites

# Installation guide
## 1. Install required packages
AMD64

```bash
sudo apt update && \
sudo apt install curl git jq build-essential gcc unzip wget lz4 git make protobuf-compiler -y
```

ARM64
```bash
sudo apt-get update
sudo apt install curl git jq build-essential gcc unzip wget lz4 git make protobuf-compiler -y
sudo apt-get install bison ed gawk gcc libc6-dev make wget
```

## 2. Install Go

AMD64

```bash
cd $HOME && \
ver="1.22.0" && \
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz" && \
sudo rm -rf /usr/local/go && \
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz" && \
rm "go$ver.linux-amd64.tar.gz" && \
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bash_profile && \
source $HOME/.bash_profile && \
go version
```

ARM64
https://akashrajpurohit.com/blog/installing-the-latest-version-of-golang-on-your-raspberry-pi/

```bash
cd $HOME && \
ver="1.22.0" && \
wget "https://dl.google.com/go/go$ver.linux-arm64.tar.gz" && \
sudo rm -rf /usr/local/go && \
sudo tar -C /usr/local -xzf "go$ver.linux-arm64.tar.gz" && \
rm "go$ver.linux-arm64.tar.gz" && \
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bash_profile && \
source $HOME/.bash_profile && \
go version
```

## 3. Install carv binary
```bash
git clone https://github.com/carv-protocol/verifier
cd verifier
git checkout tags/v0.1.1
make build
make init
go get google.golang.org/grpc
make all
```

## 4. Set up variables
```bash
# Customize if you need
echo 'export CARV_NODE_PRIVATE_KEY="private_wallet_key"' >> ~/.bash_profile
source $HOME/.bash_profile
```

## 5. Initialize the node
```bash
cd $HOME/verifier/bin
./verifier -conf ../configs/config.yaml -private-key $CARV_NODE_PRIVATE_KEY
```
