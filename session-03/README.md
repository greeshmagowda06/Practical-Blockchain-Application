# Session 03 — Build a Blockchain API

**Course:** Introduction to Smart Contracts with Solidity  
**University:** Atria University  
**Name:** V. Greeshma Gowda  
**Enrolment ID:** AU23UG019

## 1. What this project does

This Python service exposes read-only FastAPI endpoints backed by Web3.py and an Ethereum Sepolia RPC connection. It reports blockchain network status, reads the configured MessageBoard contract message, and retrieves an Ethereum wallet balance in both wei and ETH.

The ABI maintained in `contract.py` describes the `getMessage()` and `setMessage(string)` functions, although the current API provides only read operations.

## 2. Design decisions

The service uses Python, FastAPI, Pydantic response modeling, Web3.py, and `python-dotenv`.

Application startup validates the RPC connection and checks for Sepolia chain ID `11155111`. The configured contract address is converted into checksum format before interacting with the blockchain.

Endpoint-level exceptions are handled and translated into appropriate HTTP responses:

- `503` — Blockchain connection or service unavailable.
- `400` — Invalid Ethereum wallet address.
- `503` — Contract message or balance retrieval failure.

This approach separates blockchain connectivity issues from invalid user input and provides meaningful API error responses.

## 3. Deployment

- **Network:** Ethereum Sepolia
- **Chain ID:** 11155111
- **Contract address:** Not recorded
- **Transaction hash:** Not applicable to this read-only API.
- **Method:** Run the FastAPI application locally using Uvicorn and the configured environment variables.
- **Environment:** `SEPOLIA_RPC_URL` and `CONTRACT_ADDRESS`

## 4. How to test it

Prerequisite: Python, a working Ethereum Sepolia RPC endpoint, and a deployed MessageBoard contract.

From PowerShell:

```powershell
cd session-03
py -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
Copy-Item .env.example .env
uvicorn main:app --reload
```

Open Swagger UI:

`http://127.0.0.1:8000/docs`

**Testing steps:**

1. Execute `GET /status` to check the blockchain connection.
2. Execute `GET /message` to retrieve the stored contract message.
3. Execute `GET /balance/{address}` using a valid Ethereum wallet address.
4. Verify the returned wallet balance in wei and ETH.

**Expected results:**

- Blockchain connection status and Sepolia chain ID.
- Latest block information.
- Current MessageBoard contract message.
- Checksum-formatted Ethereum address.
- Wallet balance in wei and ETH.

**Negative test:** Call `/balance/not-an-address`. The expected response is HTTP `400` with the message `Invalid Ethereum address`.

## 5. What I found difficult

The application startup depends on a reachable RPC endpoint and a deployed smart contract with an ABI matching the configured contract address.

Another challenge is handling RPC timeout and connection failures separately from invalid user input. Proper exception handling is necessary to ensure that blockchain connectivity problems are returned as meaningful HTTP errors.

Deployment values are not recorded in the source.

## 6. Acknowledgements

This work uses Python, FastAPI, Pydantic, Web3.py, Uvicorn, and Ethereum Sepolia documentation.

ChatGPT and GitHub Copilot may be used for learning, debugging assistance, and documentation.