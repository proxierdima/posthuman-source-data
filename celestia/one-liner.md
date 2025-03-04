# Celestia Node Setup Script by PostHuman Validator

[This script](https://github.com/Validator-POSTHUMAN/celestia-oneliner) is provided by PostHuman Validator to automate the setup and management of Celestia nodes. It simplifies installation, configuration, and maintenance, ensuring an efficient setup process.

## Getting Started

To download, make the script executable, and run it, use the following command:

```bash
curl -o celestia-manager.sh https://raw.githubusercontent.com/Validator-POSTHUMAN/celestia-oneliner/main/celestia-manager.sh && chmod +x celestia-manager.sh && ./celestia-manager.sh
```

## Features

### 1. Install Node (Full Setup)
- Checks system requirements (CPU, RAM, and disk space) for compatibility.
- Installs necessary dependencies like Go and other libraries.
- Downloads and installs Celestia binaries.
- Initializes the node on the Celestia network.
- Configures the node with default settings, seeds, and peers.
- Sets up systemd services to run Celestia as a background service.

### 2. Install Snapshot for Faster Sync
- Provides options for installing a "Pruned" or "Archive" snapshot to speed up initial synchronization.
- Downloads and extracts the selected snapshot.
- Automatically backs up the `priv_validator_state.json` file and restores it after extraction.
- Restarts the services to resume node operation.

### 3. Update Node
- Downloads and installs the latest Celestia binaries.
- Restarts the node services to apply the updates.

### 4. Create Validator
- Initializes the node with a unique moniker.
- Exports validator and EVM keys for wallet integration.
- Ensures the node is fully synchronized before proceeding with validator creation.

### 5. Validator Operations
- **View Validator Info**: Displays details about the validator.
- **Delegate Tokens**: Enables staking of tokens to the validator.
- **Unstake Tokens**: Allows withdrawal of staked tokens.
- **Set Withdrawal Address**: Configures the address for receiving rewards.
- **Unjail Validator**: Restores a jailed validator back to active status.

### 6. Node Operations
- **Node Info**: Displays the current status and configuration of the node.
- **Your Node Peer**: Shows the node's peer connection details.
- **Firewall Configuration**: Sets up necessary firewall rules.
- **Delete Node**: Removes the node's data and configuration.

### 7. Service Operations
- **Start, Stop, Restart Service**: Controls Celestia services.
- **Check Logs**: Monitors Celestia service logs in real time.
- **Enable/Disable Service**: Configures automatic startup behavior.

### 8. Bridge Node Setup
- **Install Bridge Node**: Initializes a Celestia bridge node.
- **Bridge Node Wallet**: Displays wallet information for bridge transactions.
- **Update Bridge Node**: Updates the bridge node to the latest version.
- **Reset Bridge Node**: Resets the bridge node for troubleshooting.

### 9. Advanced Operations
- **Toggle RPC & gRPC**: Enables or disables public RPC and gRPC endpoints.
- **Toggle API**: Controls API accessibility.
- **Check Sync Status**: Monitors block synchronization progress.

## System Requirements

Minimum system requirements for running a Celestia node:
- **CPU**: 8 cores
- **RAM**: 24 GB
- **Disk Space**: 3 TB (for archive node)

## License
This script is provided by [PostHuman Validator](https://posthuman.digital).
