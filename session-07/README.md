# Session 07 — Custodial Keys & Transaction Services

**Course:** Introduction to Smart Contracts with Solidity  
**University:** Atria University  
**Name:** V. Greeshma Gowda  
**Enrolment ID:** AU23UG019

## 1. What this project does

This FastAPI service controls a backend Ethereum account and updates a MessageBoard smart contract on behalf of API callers.

It provides service-status information, ABI-encodes `setMessage(string)`, prepares EIP-1559 transactions, and supports signing, broadcasting, and waiting for transaction confirmation.

The application provides different API endpoints for checking blockchain connectivity, encoding contract calls, preparing transactions, and submitting signed transactions to the Ethereum network.

## 2. Design decisions

The backend uses Python, FastAPI, Web3.py, eth-account, Pydantic, and `python-dotenv`.

`contract.py` defines the contract ABI, while `main.py` manages the blockchain connection and transaction workflow.

The application loads the private key from environment configuration, retrieves the pending account nonce, estimates gas with a 20% margin, and calculates `maxFeePerGas` and `maxPriorityFeePerGas`.

Transactions are signed locally before being broadcast to the network.

The service returns transaction information, including the transaction hash, nonce, status, and block number after confirmation.

The implemented endpoints are:

- `GET /status`
- `POST /encode`
- `POST /prepare`
- `POST /send`

The POST endpoints use a JSON request body containing a message where applicable:

```json
{
  "message": "Hello Blockchain"
}
```

## 3. Deployment

- **Network:** Read from the configured RPC; the course topic targets Ethereum Sepolia.
- **Chain ID:** Read at runtime from the connected RPC endpoint.
- **Contract address:** Not recorded.
- **Transaction hashes:** Not recorded.
- **Method:** Run the FastAPI service with a backend-controlled Ethereum account.
- **Environment:** `SEPOLIA_RPC_URL`, `PRIVATE_KEY`, and `CONTRACT_ADDRESS`.

No transaction has been broadcast during this documentation task.

## 4. How to test it

Prerequisite: Python, a working Ethereum Sepolia RPC endpoint, a deployed MessageBoard contract, and a funded test account.

From PowerShell:

```powershell
cd session-07\backend
py -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
Copy-Item .env.example .env
uvicorn main:app --reload
```

Open Swagger UI:

`http://127.0.0.1:8000/docs`

**Testing steps:**

1. Execute `GET /status` to retrieve the service account, wallet balance, chain ID, and current contract message.
2. Execute `POST /encode` to generate the contract transaction calldata and function selector.
3. Execute `POST /prepare` to prepare a transaction with estimated gas and EIP-1559 fee information.
4. Execute `POST /send` to sign and broadcast the transaction.

The `/send` endpoint waits up to 120 seconds for transaction confirmation and returns the receipt details.

**Expected results:**

- Service account address and balance.
- Connected blockchain network and chain ID.
- Current MessageBoard message.
- Encoded transaction calldata.
- Prepared transaction details.
- Transaction hash and nonce.
- Confirmation status and block number.

**Negative test:** Submit an invalid message transaction or use a backend wallet without sufficient funds. Gas simulation or node submission should return an explicit HTTP error instead of a success-shaped response.

## 5. What I found difficult

Custodial transaction signing requires careful handling of private keys, nonce coordination, gas estimation, fee selection, node rejection, and transaction receipt timeouts.

Another important consideration is ensuring that private keys remain protected while the backend performs signing operations on behalf of API callers.

The service logs the account address but keeps the private key in environment configuration.

Deployment values and audit-log storage are not recorded in the source.

## 6. Acknowledgements

This work uses Python, FastAPI, Web3.py, eth-account, EIP-1559 transaction concepts, and Ethereum documentation.

ChatGPT and GitHub Copilot may be used for learning, debugging assistance, and documentation.