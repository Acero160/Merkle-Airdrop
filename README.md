# 🥯 Merkle Airdrop with Signature Verification

This project demonstrates how to build a secure and efficient **ERC20 airdrop mechanism** using a **Merkle Tree** and **EIP-712 signature verification**. It is implemented in Solidity and tested using Foundry.

## 🔍 Overview

This system allows project owners to distribute ERC20 tokens (BagelToken) to eligible users, validated through:

- **Merkle Proofs**: To verify that an address is in the predefined whitelist.
- **Off-chain Signatures (EIP-712)**: To ensure claims are only initiated by the rightful recipient.

Combining Merkle trees and signature validation improves both **security** and **gas efficiency** in airdrop campaigns.

---

## 🧱 Components

### Contracts

| File | Description |
|------|-------------|
| [`BagelToken.sol`](./src/BagelToken.sol) | A simple `ERC20` token contract used for airdrop distribution. |
| [`MerkleAirdrop.sol`](./src/MerkleAirdrop.sol) | Main airdrop contract that verifies Merkle proofs and EIP-712 signatures. |

### Scripts

| File | Description |
|------|-------------|
| [`DeployMerkleAirdrop.s.sol`](./script/DeployMerkleAirdrop.s.sol) | Deploys the `BagelToken` and `MerkleAirdrop` contracts and transfers tokens. |
| [`GenerateInput.s.sol`](./script/GenerateInput.s.sol) | Generates the input JSON file for the Merkle Tree with user data and writes it to disk. |
| [`Interact.s.sol`](./script/Interact.s.sol) | Script to simulate claiming the airdrop using a Merkle proof and EIP-712 signature. |

---

## 📦 Tech Stack

- **Solidity `^0.8.24`**
- [OpenZeppelin Contracts](https://github.com/OpenZeppelin/openzeppelin-contracts)
- [Foundry](https://book.getfoundry.sh/) (`forge-std`, `DevOpsTools`)

---

## 🧪 How It Works

1. **Generate Airdrop Input**
   - Create a list of eligible users and their token amounts using `GenerateInput.s.sol`.

2. **Build Merkle Tree**
   - Off-chain, compute the Merkle root using a script (not included here) and deploy the `MerkleAirdrop` contract with it.

3. **Deploy Contracts**
   - Use `DeployMerkleAirdrop.s.sol` to deploy `BagelToken` and `MerkleAirdrop`, and transfer tokens to the airdrop contract.

4. **Claim Tokens**
   - Eligible users call the `claim()` function by providing:
     - Their address
     - Amount
     - Merkle proof
     - EIP-712 signature

   - The contract validates:
     - The Merkle proof
     - That the account has not claimed already
     - That the signature is valid

---


## ✅ Security Features

- ✅ **One-time Claim**: Prevents double claiming using a mapping.  
- ✅ **Valid Signature Check**: Prevents unauthorized claims.  
- ✅ **Gas-optimized Proofs**: Only the proof and signature are required.  

---

## 🛠 Setup & Usage

### Requirements

- [Foundry](https://book.getfoundry.sh/getting-started/installation)

### Build & Test

```bash
forge build
forge test
````
---

## ⚠️Notes
- The signature used for claiming must be regenerated if the contract is redeployed, as the EIP-712 domain hash changes.

- You must compute the Merkle root off-chain and pass it to the constructor.
