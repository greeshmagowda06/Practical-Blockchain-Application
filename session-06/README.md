# Session 06 — Wallet Basics — Keys, Addresses & Digital Signatures

**Course:** Introduction to Smart Contracts with Solidity  
**University:** Atria University  
**Name:** V. Greeshma Gowda  
**Enrolment ID:** AU23UG019

## 1. What this project does

This project demonstrates Ethereum account creation, public and private keys, wallet addresses, mnemonic-based account derivation, digital message signing and verification, and the construction and broadcasting of a raw EIP-1559 transaction.

The four Python scripts cover different aspects of Ethereum wallet functionality. The first three scripts demonstrate account generation, mnemonic derivation, and message signing and recovery locally. The fourth script, `04_raw_transaction.py`, connects to an Ethereum Sepolia RPC endpoint and sends `0.001` ETH to a configured recipient.

## 2. Design decisions

The project uses Python, `eth-account`, `eth-keys`, `mnemonic`, Web3.py, and `python-dotenv`.

Hierarchical deterministic (HD) accounts are derived using standard derivation paths such as `m/44'/60'/0/0/0`.

Message signing and verification use an EIP-191-style defunct message. The verification process recovers the signer's Ethereum address from the generated digital signature.

The raw transaction implementation obtains the pending account nonce, chain ID, base fee, and priority fee. It constructs an EIP-1559 transaction, signs it locally using the private key, broadcasts the signed transaction, and waits for a transaction receipt.

## 3. Deployment

- **Network:** The raw transaction reads the configured network; the intended network is Ethereum Sepolia.
- **Chain ID:** Read at runtime from the configured RPC endpoint.
- **Contract address:** Not applicable.
- **Transaction hash:** Not recorded.
- **Method:** `04_raw_transaction.py` signs and broadcasts an ETH value-transfer transaction.
- **Environment:** `SEPOLIA_RPC_URL`, `PRIVATE_KEY`, and `RECIPIENT_ADDRESS`.

No transaction hash or confirmation details have been recorded.

**Security warning:** Do not execute the raw transaction script using a valuable private key or an unintended recipient address. Always review the transaction amount and network before broadcasting.

## 4. How to test it

Prerequisite: Python, the required dependencies, and an Ethereum Sepolia RPC endpoint for the raw transaction demonstration.

From PowerShell:

```powershell
cd session-06
py -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
python script\01_make_address.py
python script\02_seed_phrase.py
python script\03_sign_verify.py
```

The first three scripts should demonstrate:

- Generation of Ethereum wallet addresses.
- Public and private key creation.
- Mnemonic-based account derivation.
- Digital message signing.
- Signature verification and address recovery.

**Raw transaction testing:**

Copy `.env.example` to `.env`, configure the required environment variables, review the recipient address and transaction amount, and run:

```powershell
python script\04_raw_transaction.py
```

Expected output includes signed raw transaction data and a transaction receipt if the transaction is intentionally broadcast and successfully confirmed.

**Negative test:** Alter the original message after signing and attempt signature recovery. The recovered address should no longer match the original signer.

## 5. What I found difficult

The main challenges include handling private keys and mnemonic phrases securely, following the correct HD wallet derivation path, understanding the difference between digital message signatures and transaction signatures, and selecting compatible EIP-1559 fee and nonce values.

Another important consideration is ensuring that the correct blockchain network and recipient address are configured before broadcasting a transaction.

The scripts print sensitive demonstration material, so they should only be used with disposable local or test accounts.

## 6. Acknowledgements

This work uses Python, eth-account, eth-keys, mnemonic, Web3.py, and Ethereum account and digital-signing documentation.

ChatGPT and GitHub Copilot may be used for learning, debugging assistance, and documentation.