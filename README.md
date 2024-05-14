# Running Initia Node

Overview
Operating an Initia node demands significant server resources and is primarily necessary for specific tasks, such as developing dApps or functioning as validators. 

Prerequisites
Hardware/OS Requirements
The minimum hardware requirements for running an Initia node are:

| Komponen | Spesifikasi minim |
|---------|------------------------|
| CPU: 4 cores
| Memory: 16GB RAM
| Disk: 1 TB SSD Storage
| Bandwidth: 100 Mbps

```
sudo apt update && \
sudo apt install curl git jq build-essential gcc unzip wget lz4 -y
```
```
cd $HOME && \
ver="1.22.0" && \
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz" && \
sudo rm -rf /usr/local/go && \
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz" && \
rm "go$ver.linux-amd64.tar.gz" && \
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> ~/.bash_profile && \
source ~/.bash_profile && \
go version
```
```
cd initia
git checkout v0.2.11
make install
```
