# Running Initia Node

Overview
Operating an Initia node demands significant server resources and is primarily necessary for specific tasks, such as developing dApps or functioning as validators. 

Prerequisites
Hardware/OS Requirements
The minimum hardware requirements for running an Initia node are:

| Komponen | Hardware/OS Requirements |
|---------|------------------------|
| CPU: 4 cores
| Memory: 16GB RAM
| Disk: 1 TB SSD Storage
| Bandwidth: 100 Mbps

#Update 
```
sudo apt update && \
sudo apt install curl git jq build-essential gcc unzip wget lz4 -y
```
#Open Port
```
sudo apt install -y ufw
sudo ufw default allow outgoing
sudo ufw default deny incoming
sudo ufw allow ssh
sudo ufw allow 1317
sudo ufw allow 26656
sudo ufw enable

```
#Install Go

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
#Clone repo
```
git clone https://github.com/initia-labs/initia.git
cd initia
git checkout v0.2.11
make install

```
#cek version
```
initiad version --long

```
commit: fdb6df3c86fe6911cc02830b4dd59ac0ea4aedea
cosmos_sdk_version: v0.0.0-20240425031032-6bc18cf6e67d
go: go version go1.22.3 linux/amd64
name: initia
server_name: initiad
version: v0.2.11

#Services
```
sudo tee /etc/systemd/system/initiad.service > /dev/null << EOF
[Unit]
Description=initia node
After=network-online.target
[Service]
User=$USER
ExecStart=$(which initiad) start
Restart=on-failure
RestartSec=10
LimitNOFILE=10000
[Install]
WantedBy=multi-user.target
EOF

```
#reload
```
sudo systemctl daemon-reload
sudo systemctl enable initiad
sudo systemctl start initiad

```
#Run Node
```
sudo journalctl -fu initiad -o cat

```
#Create Node
```
initiad init [moniker] --chain-id initiation-1

```
Moniker change to your moniker example 
initiad init ditsy --chain-id initiation-1

#get genesis.json
```
wget https://initia.s3.ap-southeast-1.amazonaws.com/initiation-1/genesis.json
cp genesis.json ~/.initia/config/genesis.json

```

#SEED

```
sed -i -e 's/external_address = \"\"/external_address = \"'$(curl httpbin.org/ip | jq -r .origin)':26656\"/g' ~/.initia/config/config.toml

```

# setup validator

```
initiad tx mstaking create-validator \
    --amount=5000000uinit \   # It can be other LP tokens 
    --pubkey=$(initiad tendermint show-validator) \
    --moniker="<your_moniker>" \
    --chain-id=<chain_id> \
    --from=<key_name> \
    --commission-rate="0.10" \
    --commission-max-rate="0.20" \
    --commission-max-change-rate="0.01" \
    --identity=<keybase_identity> # (optional) for validator image

```

#PEERS

```
093e1b89a498b6a8760ad2188fbda30a05e4f300@35.240.207.217:26656

```

#SEED

```
2eaa272622d1ba6796100ab39f58c75d458b9dbc@34.142.181.82:26656
c28827cb96c14c905b127b92065a3fb4cd77d7f6@testnet-seeds.whispernode.com:25756

```

#Run Node
```
sudo journalctl -fu initiad -o cat

```
