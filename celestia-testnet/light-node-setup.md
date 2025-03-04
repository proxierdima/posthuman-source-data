PostHuman Celestia Light Node Setup Guide
This guide will help you set up a Celestia light node using PostHuman infrastructure.

## Hardware Requirements
| Node Type           | Memory       | Disk        |
|---------------------|-------------|------------|
| Light node        | 500 MB RAM   | 100 GB SSD  |
| Bridge node       | 64 GB RAM    | 5 TiB NVME  |
| Full storage node | 64 GB RAM    | 5 TiB NVME  |

## 1. Update Packages and Install Dependencies
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install curl git wget htop tmux build-essential jq make gcc tar clang pkg-config libssl-dev ncdu -y
```

## 2. Install Go
```bash
cd ~
! [ -x "$(command -v go)" ] && {
  VER="1.22.6"
  wget "https://golang.org/dl/go$VER.linux-amd64.tar.gz"
  sudo rm -rf /usr/local/go
  sudo tar -C /usr/local -xzf "go$VER.linux-amd64.tar.gz"
  rm "go$VER.linux-amd64.tar.gz"
  [ ! -f ~/.bash_profile ] && touch ~/.bash_profile
  echo "export PATH=$PATH:/usr/local/go/bin:~/go/bin" >> ~/.bash_profile
  source ~/.bash_profile
}
[ ! -d ~/go/bin ] && mkdir -p ~/go/bin
go version
```

## 3. Install Celestia Node
```bash
cd $HOME
rm -rf celestia-node
git clone https://github.com/celestiaorg/celestia-node.git
cd celestia-node/
git checkout tags/v0.21.5
make build
sudo make install
make cel-key
```

## 4. Configure and Initialize the Light Node
```bash
celestia light init
```

## 5. Create a Wallet
```bash
KEY_NAME="my_celes_key"
cd ~/celestia-node
./cel-key add $KEY_NAME --keyring-backend test --node.type light
```

### (Optional) Restore an Existing Wallet
```bash
KEY_NAME="my_celes_key"
cd ~/celestia-node
./cel-key add $KEY_NAME --keyring-backend test --node.type light --recover
```

Retrieve your wallet address using:
```bash
cd $HOME/celestia-node
./cel-key list --node.type light --keyring-backend test
```

## 6. Add Consensus Node RPC and gRPC Ports
```bash
CORE_IP="<PUT_CONSENSUS_NODE_IP>"
CORE_RPC_PORT="<PUT_CONSENSUS_NODE_RPC_PORT>"
CORE_GRPC_PORT="<PUT_CONSENSUS_NODE_GRPC_PORT>"
KEY_NAME="my_celes_key"
```

## 7. Create a Service File for Celestia Light
```bash
sudo tee /etc/systemd/system/celestia-light.service > /dev/null <<EOF
[Unit]
Description=Celestia Light Node
After=network-online.target

[Service]
User=$USER
ExecStart=$(which celestia) light start \
--core.ip $CORE_IP \
--core.rpc.port $CORE_RPC_PORT \
--core.grpc.port $CORE_GRPC_PORT \
--keyring.accname $KEY_NAME \
--metrics.tls=true \
--metrics --metrics.endpoint otel.celestia.observer
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

Enable and start the service:
```bash
sudo systemctl daemon-reload
sudo systemctl enable celestia-light
sudo systemctl restart celestia-light && sudo journalctl -u celestia-light -fo cat
```

## 8. Retrieve Node Information
Generate an auth token:
```bash
NODE_TYPE=light
AUTH_TOKEN=$(celestia $NODE_TYPE auth admin)
```

Get the peer ID:
```bash
curl -X POST \
     -H "Authorization: Bearer $AUTH_TOKEN" \
     -H 'Content-Type: application/json' \
     -d '{"jsonrpc":"2.0","id":0,"method":"p2p.Info","params":[]}' \
     http://localhost:26658
```

## 9. Useful Commands (Cheat Sheet)

### Check Wallet Balance
```bash
celestia state balance --node.store ~/.celestia-light/
```

### Get Wallet Address
```bash
cd $HOME/celestia-node
./cel-key list --node.type light --keyring-backend test
```

### Restore an Existing Key
```bash
KEY_NAME="my_celes_key"
cd ~/celestia-node
./cel-key add $KEY_NAME --keyring-backend test --node.type light --recover
```

### Check Node Sync Status
```bash
celestia header sync-state --node.store ~/.celestia-light/
```

### Get Node ID
```bash
celestia p2p info --node.store ~/.celestia-light/
```

### Add Permissions for Key Transfers
```bash
chmod -R 700 ~/.celestia-light
```

### Reset Node
```bash
celestia light unsafe-reset-store
```

## 10. Upgrade Instructions

### Stop Light Node
```bash
sudo systemctl stop celestia-light
```

### Download Latest Version
```bash
cd $HOME
rm -rf celestia-node
git clone https://github.com/celestiaorg/celestia-node.git
cd celestia-node/
git checkout tags/v0.21.5
make build
sudo make install
make cel-key
```

### Update Configuration
```bash
celestia light config-update
```

### Restart Light Node
```bash
sudo systemctl restart celestia-light && sudo journalctl -u celestia-light -fo cat
```

## 11. Delete Light Node
```bash
sudo systemctl stop celestia-light
sudo systemctl disable celestia-light
sudo rm /etc/systemd/system/celestia-light*
rm -rf $HOME/celestia-node $HOME/.celestia-light
```

## 12. External Explorer
View node information here:
🔗 Celestia PostHuman Explorer

This guide is customized for PostHuman Celestia Mainnet using the following endpoints:

- **Mainnet Type:** mainnet
- **Chain ID:** celestia
- **RPC:** https://rpc.celestia-mainnet.posthuman.digital
- **REST:** https://rest.celestia-mainnet.posthuman.digital
- **gRPC:** https://grpc.celestia-mainnet.posthuman.digital
- **Peer:** cd9f852141cd6f78e9443cea389911a6f0a5df72@8.52.247.252:26656

🚀 Your Celestia Light Node is now set up and running on PostHuman infrastructure!

**POSTHUMAN © Copyright 2025. All Rights Reserved.**

