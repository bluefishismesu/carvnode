# Carv Node Installation Guide

This guide will walk you through downloading and installing a specific version of the Go compiler environment on Ubuntu 20.04 or Raspberry Pi, and compiling the Carv node.

## Prerequisites

# Installation Guide
## 1. Install Required Packages
### AMD64

```bash
sudo apt update && \
sudo apt install curl git jq build-essential gcc unzip wget lz4 git make protobuf-compiler -y
```

### ARM64(raspberry-pi5)
```bash
sudo apt-get update
sudo apt-get upgrade -y
sudo apt install curl git jq build-essential gcc unzip wget lz4 git make protobuf-compiler -y
sudo apt-get install bison ed gawk gcc libc6-dev make wget -y
```

## 2. Install Go

### AMD64

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

### ARM64(raspberry-pi5)
Refer to this link for detailed instructions: 
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

## 3. Install carv binary(alphanet) 
For detailed instructions, refer to: 
https://docs.carv.io/carv-protocol/verifier-node-explained/join-alphanet-verifier-nodes/operating-a-verifier-node/running-in-cli/using-source-code


```bash
git clone https://github.com/carv-protocol/verifier
cd verifier
git checkout verifier-alphanet
make build
make init
go get google.golang.org/grpc
make all
```

## 4. Set up variables
Replace private_wallet_key with your burn wallet private key. To get your private key from Metamask, follow the instructions here:
https://support.metamask.io/managing-my-wallet/secret-recovery-phrase-and-private-keys/how-to-export-an-accounts-private-key/


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



## 6. Create Service
Create the service file:
```bash
sudo nano /etc/systemd/system/verifier.service
```

Service Contents:

```bash
[Unit]
Description=Verifier Service
After=network.target

[Service]
User=pi
EnvironmentFile=/home/pi/.bash_profile
WorkingDirectory=/home/pi/verifier/bin
ExecStart=/home/pi/verifier/bin/verifier -conf /home/pi/verifier/configs/config.yaml -private-key $CARV_NODE_PRIVATE_KEY
Restart=always
StandardOutput=file:/var/log/verifier.log
StandardError=file:/var/log/verifier-error.log

[Install]
WantedBy=multi-user.target
```

Restart and enable the service:

```bash
sudo systemctl daemon-reload
sudo systemctl enable verifier.service
sudo systemctl start verifier.service
```

Check the service status:

```bash
sudo systemctl status verifier.service
```
