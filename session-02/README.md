# Session 02 — Build a Wallet-Connected Page

**Course:** Introduction to Smart Contracts with Solidity  
**University:** Atria University  
**Name:** V. Greeshma Gowda  
**Enrolment ID:** AU23UG019

## 1. What this project does

This Next.js browser dApp connects to MetaMask, displays the selected account and chain ID, reads `MessageBoard.getMessage()`, and writes a new message through `setMessage(string)`.

The page displays idle, pending, confirmed, and failed transaction states and provides a link to Sepolia Etherscan for viewing a confirmed transaction.

## 2. Design decisions

The frontend uses Next.js 16, React 19, TypeScript, and ethers.js 6.

`app/page.tsx` uses `BrowserProvider` to connect to the injected wallet, `Contract` for reading contract data and signing write transactions, and event listeners for `accountsChanged` and `chainChanged`.

The contract ABI is maintained separately in `lib/contract.ts`, while the contract address is configured through `NEXT_PUBLIC_CONTRACT_ADDRESS`.

Write operations are restricted to chain ID `11155111` (Ethereum Sepolia) and wait for a successful transaction receipt before refreshing the contract state.

## 3. Deployment

- **Network:** Ethereum Sepolia
- **Chain ID:** 11155111
- **Contract address:** Not recorded
- **Transaction hash:** Not recorded
- **Explorer URL:** Generated from the runtime transaction hash; no transaction hash is recorded.
- **Method:** Run the Next.js development server and interact with the application through MetaMask in the browser.
- **Environment:** `NEXT_PUBLIC_CONTRACT_ADDRESS`

No deployment or transaction has been broadcast yet.

## 4. How to test it

Prerequisite: Node.js, a browser with MetaMask installed, and a Sepolia test account.

From PowerShell:

```powershell
cd session-02
Copy-Item .env.example .env.local
npm install
npm run dev
```

Open `http://localhost:3000` in your browser.

**Testing steps:**

1. Connect MetaMask to the application.
2. Select the Ethereum Sepolia network.
3. Click **Get Message** to read the existing contract message.
4. Enter a non-empty message.
5. Click **Update Message**.
6. Confirm the transaction in MetaMask.
7. Wait for transaction confirmation and verify the updated message.

**Expected results:**

- Connected wallet address is displayed.
- Current chain ID is displayed.
- Existing contract message is retrieved.
- Pending state is displayed while the transaction is processing.
- Confirmed state is displayed after successful mining.
- A Sepolia Etherscan transaction link is provided.

**Negative test:** Leave the message empty or switch to another network. The application should display an appropriate error and must not submit an invalid write transaction.

## 5. What I found difficult

Handling injected-wallet availability, account changes, network changes, contract-address configuration, and asynchronous transaction confirmation requires separate UI states.

Another challenge is maintaining accurate wallet information when MetaMask changes accounts or networks. The application clears stale state when these changes occur.

The contract deployment information is not available in the source.

## 6. Acknowledgements

This work uses Next.js, React, TypeScript, ethers.js, MetaMask, and Ethereum Sepolia documentation.

ChatGPT and GitHub Copilot may be used for learning, debugging assistance, and documentation.