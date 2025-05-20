# How to Build and Document an NFT Minting dApp on Ethereum

### Overview
This guide will walk you through building and documenting an NFT (Non-Fungible Token) minting decentralized application (dApp) on the Ethereum blockchain. The guide adheres to the Google Developer Documentation Style Guide, focusing on clarity, precision, and user-centric explanations.

We'll cover:
- Writing and deploying a smart contract
- Writing and deploying a smart contract
- Explaining relevant Ethereum APIs and JSON-RPC calls

## Prerequisites

To follow this tutorial, ensure you have the following:

### Tools

- Node.js installed
- [Metamask](https://metamask.io/) wallet extension
- [Hardhat](https://hardhat.org/) (for development and deployment)
- Ethers.js
- [Solidity](https://soliditylang.org/) knowledge

### Required Knowledge

- Basics of smart contracts and blockchain
- Understanding of Ethereum gas fees and wallet addresses
- Basic knowledge of React.js and frontend state management
  
---

## 1. Smart Contract Breakdown
### Smart Contract: `NFTMinter.sol`

The smart contract uses the ERC-721 standard to mint unique tokens.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC721/extensions/ERC721URIStorage.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract NFTMinter is ERC721URIStorage, Ownable {
    uint256 public tokenCount;

    constructor() ERC721("MyNFT", "MNFT") {}

    function mintNFT(address recipient, string memory tokenURI) public onlyOwner returns (uint256) {
        tokenCount++;
        uint256 newItemId = tokenCount;
        _mint(recipient, newItemId);
        _setTokenURI(newItemId, tokenURI);
        return newItemId;
    }
}
```
### Why Use `ERC721URIStorage`?

`ERC721URIStorage` extends `ERC721` to allow per-token metadata using `tokenURI`. This is more flexible for dynamic NFTs.

### Key Features

- **ERC721URIStorage**: Allows metadata (like image URLs) to be attached to tokens.
- **Ownable**: Restricts minting to the contract owner.
- **mintNFT**: Public function callable by the owner to mint NFTs.

### Deploy Using Hardhat

```
npx hardhat compile
npx hardhat run scripts/deploy.js --network goerli
```

### Get ABI

After compiling the contract, locate `artifacts/contracts/SimpleNFT.sol/SimpleNFT.json` and copy the ABI array from there.

---

## 2. Frontend Integration Steps

### Tools

- React.js
- ethers.js
- MetaMask

### Integration Process

1. **Connect to MetaMask**:

```jsx
await window.ethereum.request({ method: 'eth_requestAccounts' });

```

1. **Set up ethers.js provider and signer**:

```jsx
import { ethers } from "ethers";

const provider = new ethers.providers.Web3Provider(window.ethereum);
const signer = provider.getSigner();

```

1. **Connect to the deployed contract**:

```jsx
const contractAddress = "<deployed_contract_address>";
const contractABI = [/* ABI array */];
const nftContract = new ethers.Contract(contractAddress, contractABI, signer);

```

1. **Call the mint function**:

```jsx
const tx = await nftContract.mintNFT(userAddress, tokenURI);
await tx.wait();

```

---

## 3. API/JSON-RPC Explanation

Ethereum provides JSON-RPC methods that power interaction with blockchain nodes. Here are a few relevant methods:

### `eth_sendTransaction`

Sends a transaction (e.g., minting NFTs) to the network.

```json
{
  "jsonrpc":"2.0",
  "method":"eth_sendTransaction",
  "params":[{
    "from": "0xYourAddress",
    "to": "0xContractAddress",
    "data": "0xFunctionSignatureAndParams"
  }],
  "id":1
}

```

### `eth_getTransactionReceipt`

Checks if the transaction was mined.
```json
{
  "jsonrpc":"2.0",
  "method":"eth_getTransactionReceipt",
  "params":["0xTransactionHash"],
  "id":1
}

```










