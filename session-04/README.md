# Session 04 — Events & Live Updates

**Course:** Introduction to Smart Contracts with Solidity  
**University:** Atria University  
**Name:** V. Greeshma Gowda  
**Enrolment ID:** AU23UG019

## 1. What this project does

`MessageBoard.sol` emits `MessageUpdated(address indexed editor, string newMessage)` whenever the stored message changes.

The backend contains a historical event scanner (`main.py`) that queries blockchain events over block ranges in chunks, and a WebSocket subscriber (`subscribe.py`) that listens for new event notifications in real time without requiring manual refreshing.

The historical scanner retrieves previously emitted events, while the subscriber monitors new blockchain activity and displays the updated message and editor address.

## 2. Design decisions

The smart contract uses an indexed editor address to support efficient event filtering.

The backend uses Python, Web3.py, FastAPI dependencies, and `python-dotenv` to interact with the Ethereum blockchain.

Historical event scanning uses ten-block chunks, retry handling for RPC rate limits, and a short delay between requests to reduce provider errors.

The live event subscriber uses `AsyncWeb3`, `WebSocketProvider`, a topic filter, and `process_log` to decode and display incoming blockchain events.

The implementation separates historical event retrieval from real-time event monitoring. The source does not contain a recovery checkpoint implementation or a browser client.

## 3. Deployment

- **Network:** Ethereum Sepolia
- **Chain ID:** 11155111
- **Contract address:** Not recorded
- **Transaction hashes:** Not recorded
- **Method:** Configure `CONTRACT_ADDRESS`, `START_BLOCK`, and the HTTP/WebSocket RPC URLs, then execute the historical scanner or live event subscriber.
- **Environment:** Backend `.env` requires `SEPOLIA_RPC_URL`, `SEPOLIA_WSS_URL`, `CONTRACT_ADDRESS`, and `START_BLOCK` as applicable.

No deployment or transaction has been broadcast yet.

## 4. How to test it

Prerequisite: Python, a working Ethereum Sepolia HTTP/WebSocket RPC endpoint, and a deployed MessageBoard contract.

From PowerShell:

```powershell
cd session-04\backend
py -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
Copy-Item .env.example .env
python main.py
```

The historical scanner should report each scanned block range and display the decoded events.

**Live event testing:**

Open a second terminal and run:

```powershell
python subscribe.py
```

After an on-chain message update, the subscriber should automatically print the new message and the editor's wallet address.

**Expected results:**

- Historical blockchain events are retrieved.
- Block ranges are scanned in chunks of ten blocks.
- Event details are decoded using the contract ABI.
- New message updates are detected through WebSocket subscriptions.
- Editor address and updated message are displayed.

**Negative test:** Set `START_BLOCK` higher than the latest available block. The `main.py` script must stop with its validation error instead of scanning an invalid block range.

## 5. What I found difficult

Working with blockchain events requires matching the deployed contract ABI, selecting appropriate block ranges, handling RPC provider rate limits, and configuring a WebSocket endpoint for live event delivery.

Another challenge is distinguishing historical event retrieval from real-time event monitoring and ensuring that incoming event logs are decoded correctly.

The source includes retry logic for HTTP historical scans, but deployment and recovery checkpoint values are not recorded.

## 6. Acknowledgements

This work uses Solidity events, Python, Web3.py, WebSocket subscriptions, and Ethereum Sepolia documentation.

ChatGPT and GitHub Copilot may be used for learning, debugging assistance, and documentation.