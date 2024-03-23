# ZkSdk 

![Era Logo](https://github.com/matter-labs/era-contracts/raw/main/eraLogo.svg)  
  

## 📌 Overview

To begin, it is useful to have a basic understanding of the types of objects available and what they are responsible for, at a high level:

-   `Provider` provides connection to the zkSync Era blockchain, which allows querying the blockchain state, such as account, block or transaction details,
    querying event logs or evaluating read-only code using call. Additionally, the client facilitates writing to the blockchain by sending
    transactions.
-   `Wallet` wraps all operations that interact with an account. An account generally has a private key, which can be used to sign a variety of
    types of payloads. It provides easy usage of the most common features.

## 🛠 Prerequisites

-   `node: >= 18` ([installation guide](https://nodejs.org/en/download/package-manager))
-   `ethers: ^6.7.1`

## 📥 Installation & Setup

```bash
yarn add zksdk
yarn add ethers@6 # ethers is a peer dependency of zksdk
```

## 📝 Examples

### Connect to the zkSync Era network:

```ts
import { Provider, utils, types } from "zksdk";
import { ethers } from "ethers";

const provider = Provider.getDefaultProvider(types.Network.Sepolia); // zkSync Era testnet (L2)
const ethProvider = ethers.getDefaultProvider("sepolia"); // Sepolia testnet (L1)
```

### Get the latest block number

```ts
const blockNumber = await provider.getBlockNumber();
```

### Get the latest block

```ts
const block = await provider.getBlock("latest");
```

### Create a wallet

```ts
const PRIVATE_KEY = process.env.PRIVATE_KEY;
const wallet = new Wallet(PRIVATE_KEY, provider, ethProvider);
```

### Check account balances

```ts
const ethBalance = await wallet.getBalance(); // balance on zkSync Era network

const ethBalanceL1 = await wallet.getBalanceL1(); // balance on Sepolia network
```

### Transfer funds

Transfer funds among accounts on L2 network.

```ts
const receiver = Wallet.createRandom();

const transfer = await wallet.transfer({
    to: receiver,
    token: utils.ETH_ADDRESS,
    amount: ethers.parseEther("1.0"),
});
```

### Deposit funds

Transfer funds from L1 to L2 network.

```ts
const deposit = await wallet.deposit({
    token: utils.ETH_ADDRESS,
    amount: ethers.parseEther("1.0"),
});
```

### Withdraw funds

Transfer funds from L2 to L1 network.

```ts
const withdrawal = await wallet.withdraw({
    token: utils.ETH_ADDRESS,
    amount: ethers.parseEther("1.0"),
});
```

## 🤖 Running tests

For running tests, use:

```shell
yarn test:wait # waits for local-setup to be ready
yarn test:prepare # prepares the environment (deploys token on both layers, etc.)
yarn test
```

For running test coverage, use:

```shell
yarn test:coverage
```

