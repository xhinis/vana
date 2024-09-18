# 0G

![Vana Image](https://i.imgur.com/tvAuEGr.png)

---

## Community Links

- [Vana Discord](https://discord.gg/withvana)
- [Vana Twitter](https://x.com/withvana)
- [Vana Website](https://www.vana.org/)
- [Vana Blog](https://www.vana.org/post)
- [Vana github](https://github.com/vana-com)
- [Vana Explorer](https://satori.vanascan.io/)

---

## 💻 System Requirements

| Components  | Minimum Requirements |
|-------------|----------------------|
| CPU         | 4 Cores               |
| RAM         | 8+ GB                 |
| Storage     | 400 GB SSD            |


# Ubuntu 22.04


## Setup Steps

### 1. Update and Upgrade System Packages

```bash
sudo apt update
sudo apt upgrade -y
```

### 2. Install Essential Packages

```bash
sudo apt install -y curl gnupg
```

### 3. Install Node.js and npm

Add the NodeSource repository and install Node.js:

```bash
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt install -y nodejs
```

Update npm to the latest version:

```bash
sudo npm install -g npm@10.8.3
```

### 5. Clone the Repository

Clone the repository and navigate to the project directory:

```bash
git clone https://github.com/vana-com/vana-dlp-chatgpt.git
cd vana-dlp-chatgpt
```

### 6. Configure Python Version for Poetry

Set Python 3.11 as the interpreter for Poetry:

```bash
sudo add-apt-repository ppa:deadsnakes/ppa
sudo apt update
sudo apt install python3.11 python3.11-venv python3.11-dev

```

```bash
poetry env use python3.11
```

```bash
poetry install
```

```bash
poetry run python --version
poetry check
poetry --version
```


![Version check](https://i.imgur.com/MI6TYQT.png)


### 8. Create Your `.env` File

Copy the example environment configuration file:

```bash
cp .env.example .env
```

Populate this file later with DLP-specific information.

### 9. (Optional) Install Vana CLI

To install `vanacli` system-wide:

```bash
pip install vana
```

### 10. Create a Wallet

Create a wallet using the Vana CLI:

```bash
vanacli wallet create --wallet.name default --wallet.hotkey default
```

This will create two key pairs:

- **Coldkey**: For human-managed transactions (e.g., staking).
- **Hotkey**: For validator-managed transactions (e.g., submitting scores).

Follow the prompts to set a secure password. Save the mnemonic phrases securely; you'll need these to recover your wallet if needed.

### 11. Add Satori Testnet to MetaMask

Configure MetaMask with the following details:

- **Network Name**: Satori Testnet
- **RPC URL**: https://rpc.satori.vana.org
- **Chain ID**: 14801
- **Currency**: VANA

### 12. Export Your Private Keys

Export private keys using the Vana CLI:

```bash
vanacli wallet export_private_key
```

Follow the prompts to export both the coldkey and hotkey private keys.

### 13. Import Keys to MetaMask

- Click your account icon in MetaMask and select "Import Account."
- Select "Private Key" as the import method.
- Paste the private key for your coldkey and repeat the process for your hotkey.

### 14. Fund Both Addresses with Testnet VANA

Visit [https://faucet.vana.org](https://faucet.vana.org), connect your MetaMask wallet, and request VANA for both your coldkey and hotkey addresses.

**Note**: The faucet can be used once per day. If needed, ask a VANA holder to send you some test VANA tokens.

### 15. Creating a DLP

**If you're joining an existing DLP as a validator, skip to the Validator Setup section.**

#### Generate Encryption Keys

Run the key generation script:

```bash
./keygen.sh
```

This script generates RSA key pairs for file encryption/decryption in the DLP. Follow the prompts to enter your name, email, and key expiration.

The script generates four files:

- `public_key.asc` and `public_key_base64.asc` (for UI)
- `private_key.asc` and `private_key_base64.asc` (for validators)

#### Deploy DLP Smart Contracts

Clone the DLP Smart Contract repository:

```bash
cd ..
git clone https://github.com/vana-com/vana-dlp-smart-contracts.git
cd vana-dlp-smart-contracts
```

Install dependencies:

```bash
yarn install
```

Edit the `.env` file in the `vana-dlp-smart-contracts` directory:

```bash
DEPLOYER_PRIVATE_KEY=0x... (your coldkey private key)
OWNER_ADDRESS=0x... (your coldkey address)
SATORI_RPC_URL=https://rpc.satori.vana.org
DLP_NAME=... (your DLP name)
DLP_TOKEN_NAME=... (your DLP token name)
DLP_TOKEN_SYMBOL=... (your DLP token symbol)
```

Deploy contracts:

```bash
npx hardhat deploy --network satori --tags DLPDeploy
```

Note the deployed addresses for DataLiquidityPool and DataLiquidityPoolToken.

**Optional**: Verify the contracts if you made changes to the code:

```bash
npx hardhat verify --network satori <DataLiquidityPool address>
npx hardhat verify --network satori <DataLiquidityPoolToken address> "<DLP_TOKEN_NAME>" <DLP_TOKEN_SYMBOL> <OWNER_ADDRESS>
```

If no changes were made, contracts should be verified automatically.

#### Configure the DLP Contract

Visit [https://satori.vanascan.io/address/](https://satori.vanascan.io/address/):

- Go to the "Write proxy" tab
- Connect your wallet
- Call `updateFileRewardDelay` and set it to 0
- Call `addRewardsForContributors` with 1,000,000,000,000,000,000,000 (1 million tokens)

Update the `.env` file in the `vana-dlp-chatgpt` directory:

```bash
DLP_SATORI_CONTRACT=0x... (DataLiquidityPool address)
DLP_TOKEN_SATORI_CONTRACT=0x... (DataLiquidityPoolToken address)
PRIVATE_FILE_ENCRYPTION_PUBLIC_KEY_BASE64=... (content of public_key_base64.asc)
```

### 16. Validator Setup

Follow these steps whether you're a DLP creator or joining an existing DLP. Ensure you have completed the Setup section.

**Required Information**:

For non-DLP creators, request the following from the DLP creator:

- DLP contract address (DataLiquidityPool)
- DLP token contract address (DataLiquidityPoolToken)
- Public key for the DLP validator network (`public_key.asc`)
- Base64-encoded private key for the DLP validator network (`private_key_base64.asc`)

**Setup**:

Ensure you're in the `vana-dlp-chatgpt` directory:

```bash
cd vana-dlp-chatgpt
```

If you're a non-DLP creator, edit the `.env` file with the information provided by the DLP creator:

```bash
DLP_SATORI_CONTRACT=0x... (DataLiquidityPool address)
DLP_TOKEN_SATORI_CONTRACT=0x... (DataLiquidityPoolToken address)
PRIVATE_FILE_ENCRYPTION_PUBLIC_KEY_BASE64=... (base64-encoded private key)
```

**Fund Validator with DLP Tokens**:

- **For DLP creators**: Import the DLP token to MetaMask using `<DataLiquidityPoolToken address>` and send 10 tokens to your coldkey address.

- **For non-DLP creators**: Request DLP tokens from the DLP creator and ensure they are in your coldkey address.

**Register as a Validator**:

Register your validator:

```bash
./vanacli dlp register_validator --stake_amount 10
```

For non-DLP creators, ask the DLP owner to accept your registration.

**For DLP creators**: Approve validators with:

```bash
./vanacli dlp approve_validator --validator_address=<your hotkey address from MetaMask>
```

**Run Validator Node**:

Start the validator node:

```bash
poetry run python -m chatgpt.nodes.validator
```

Monitor the logs for any errors. If set up correctly, you'll see the validator waiting for new files to verify.

### 17. Test Your Validator

**For the Public ChatGPT DLP**:

- Visit the official ChatGPT DLP UI.
- Connect your wallet (must hold some VANA).
- Follow the instructions on the UI to upload a file (submit the addFile transaction).
- Wait for your validator to process the file and write scores on-chain (verifyFile transaction).
- Check the UI for a reward claiming dialog and test claiming rewards.

**For Custom DLPs**:

- Visit the demo DLP UI.
- Connect your wallet (must hold some VANA).
- Use the gear icon to set the DLP contract address and public encryption
