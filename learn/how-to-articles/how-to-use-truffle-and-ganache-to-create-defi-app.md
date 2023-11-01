---
id: defi-app-truffle
title: DeFi App using Truffle and Ganache
keywords:
  - docs
  - apothem
  - token
  - DeFi
  - truffle
description: Use Truffle and Ganache to create DeFi App
---

# How To Use Truffle and Ganache to Create DeFi App

## 🧭 Table of contents

* [🧭 Table of contents](how-to-use-truffle-and-ganache-to-create-defi-app.md#-table-of-contents)
* [📰 Overview](how-to-use-truffle-and-ganache-to-create-defi-app.md#-overview)
  * [What you will learn](how-to-use-truffle-and-ganache-to-create-defi-app.md#what-you-will-learn)
  * [What you will do](how-to-use-truffle-and-ganache-to-create-defi-app.md#what-you-will-do)
  * [📰 About DeFi](how-to-use-truffle-and-ganache-to-create-defi-app.md#-about-defi)
* [🚀 Setting up the development environment](how-to-use-truffle-and-ganache-to-create-defi-app.md#-setting-up-the-development-environment)
  * [⚒ Starting a new Truffle Project](how-to-use-truffle-and-ganache-to-create-defi-app.md#-starting-a-new-truffle-project)
  * [⚒ Configuring XDC Mainnet and Apothem Testnet on Truffle](how-to-use-truffle-and-ganache-to-create-defi-app.md#-configuring-xdc-mainnet-and-apothem-testnet-on-truffle)
  * [⚒ Adding Testnet XDC to Development Wallet](how-to-use-truffle-and-ganache-to-create-defi-app.md#-adding-testnet-xdc-to-development-wallet)
* [💵 Creating your first DeFi App](how-to-use-truffle-and-ganache-to-create-defi-app.md#-creating-our-first-defi-app)
  * [💵 Create and Deploy XRC20 Token](how-to-use-truffle-and-ganache-to-create-defi-app.md#-create-and-deploy-xrc20-token)
  * [💵 Create FarmToken contract](how-to-use-truffle-and-ganache-to-create-defi-app.md#-create-farmtoken-contract)
  * [💵 Compiling and Deploying](how-to-use-truffle-and-ganache-to-create-defi-app.md#-compiling-and-deploying)
  * [💵 Testing FarmToken contract](how-to-use-truffle-and-ganache-to-create-defi-app.md#-testing-farmtoken-contract)
  * [💵 Deploying on a live network](how-to-use-truffle-and-ganache-to-create-defi-app.md#-deploying-on-a-live-network)
* [🔍 Veryfing Contracts on the Block Explorer](how-to-use-truffle-and-ganache-to-create-defi-app.md#-veryfing-contracts-on-the-block-explorer)

## 📰 Overview

[Truffle](https://trufflesuite.com/) is a blockchain development environment, which you can use to create and test smart contracts by leveraging an Ethereum Virtual Machine. [Ganache](https://trufflesuite.com/ganache/) is a tool to create a local blockchain for testing your smart contracts. It simulates all the features of a real blockchain network but costs you nothing to deploy and test your code.

#### What you will learn

In this tutorial, we will build a DeFi Application with Solidity where users can deposit an XRC20 token to the smart contract and it will mint and transfer Farm Tokens to them. The users can later withdraw their XRC20 tokens by burning their Farm Token with a smart contract, and the XRC20 tokens will be transferred back to them.

#### What you will do

* Install and setup Truffle and Ganache
* Create a Truffle project
* Create an XRC20 Token
* Compile an XRC20 Token
* Deploy an XRC20 Token
* Create FarmToken Smart Contract

### 📰 About DeFi

Decentralized finance, or **DeFi**, is a term which describes blockchain-based financial services. DeFi provides virtually any financial service on a blockchain in a permissionless way. You can borrow, lend, stake, trade, and more using DeFi services.

## 🚀 Setting up the development environment

There are a few technical requirements before we start. Please install the following:

* [Node.js v8+ LTS and npm](https://nodejs.org/en/) (comes with Node)
* [Git](https://git-scm.com/)

Once we have those installed, we only need one command to install Truffle:

```bash
npm install -g truffle
```

To verify that Truffle is installed properly, type **`truffle version`** on a terminal. You should see something like:

```bash
Truffle v5.5.27 (core: 5.5.27)
Ganache v7.4.0
Solidity v0.5.16 (solc-js)
Node v16.16.0
Web3.js v1.7.4
```

If you see an error instead, make sure that your npm modules are added to your path.

### ⚒ Starting a new Truffle Project

You will start by setting up tour folder. In this example, we are creating a project called `FarmToken`. You can create a new `FarmToken` folder by running on the following on terminal:

```bash
mkdir FarmToken && cd FarmToken
```

Then run `truffle init`. If Truffle is correctly installed on your local environment, you should see the following message:

```bash
Starting init...
================

> Copying project files to /home/your/path/to/FarmToken

Init successful, sweet!

Try these scaffold commands to get started:
  $ truffle create contract YourContractName # scaffold a contract
  $ truffle create test YourTestName         # scaffold a test

http://trufflesuite.com/docs
```

Your folder files will look like this:

![Step 01](https://user-images.githubusercontent.com/78161484/190839624-495ef863-e177-4c62-81ca-680e5e6a4cab.png)

### ⚒ Configuring XDC Mainnet and Apothem Testnet on Truffle

In order to get started deploying new contracts on XDC Mainnet and/or Apothem, you'll need to install two new dependencies that will be used in the `truffle-config.js` file. These dependencies are `@truffle/hdwallet-provider` and `dotenv`. First, choose your preferred package manager. In this example we are using `yarn` but you can also use `npm`.

If you never used `yarn` before, you might need to install it first.\
‼️You can skip this step if you already have yarn installed‼️

```
npm install --global yarn
```

Initialize your package manager on your folder and install the required dependencies:

```
yarn init -y
yarn add @truffle/hdwallet-provider dotenv
```

You will also need a **24-Word Mnemonic Phrase**. To configure your wallet, create a new `.env` file and write your mnemonic by running:

```
touch .env
echo MNEMONIC=arm derive cupboard decade course garlic journey blast tribe describe curve obey >> .env
```

Remember to change the **24-Word Mnemonic** above for your own mnemonic. The contents of your `.env` file should read as follow:

```jsx
MNEMONIC=arm derive cupboard decade course garlic journey blast tribe describe curve obey
```

🚨 **Do not use the mnemonic in the example above in production or you can risk losing your assets and/or the ownership of your smart contracts!** 🚨

Lastly, you can configure the `truffle-config.js` file for both Apothem and XinFin Networks by writting:

```jsx
require('dotenv').config();
const { MNEMONIC } = process.env;
const HDWalletProvider = require('@truffle/hdwallet-provider');

module.exports = {

  networks: {

    xinfin: {
      provider: () => new HDWalletProvider(
        MNEMONIC,
        'https://erpc.xinfin.network'),
      network_id: 50,
      gasLimit: 6721975,
      confirmation: 2,
    },

    apothem: {
      provider: () => new HDWalletProvider(
        MNEMONIC,
        'https://erpc.apothem.network'),
      network_id: 51,
      gasLimit: 6721975,
      confirmation: 2,
    }
  },

  mocha: {
  },

  compilers: {
    solc: {
      version: "0.8.16",
    }
  },
};
```

### ⚒ Adding Testnet XDC to Development Wallet

It is possible to list all XDC addresses bound to your mnemonic on truffle by accessing the truffle console:

```
truffle console --network xinfin
```

Once the truffle console CLI opens, you can run:

```
truffle(xinfin)> accounts
```

And the console should log all accounts bound to your mnemonic phrase as follow:

```jsx
[
  '0xA4e66f4Cc17752f331eaC6A20C00756156719519',
  '0x0431d52FE37F3839895018272dfa3bA189fcE07E',
  '0x11A6D9727c16064950473a4c8A92dC294190f7fF',
  '0x4464DDF9969E9a8e5CfF02E3706AEB4ccA92A314',
  '0xFa73bE6AA126DEC47ce14a22B7BAaF8BAFaB59Fb',
  '0xEdFFc4e7476f05f43cA3e6f5784349dE6E6373D5',
  '0x07795c732Bb013165FADCE64B884bf9971Bf9636',
  '0x5dF551A53bEaAB8bb2307eF459aA5AAFbb5F73cc',
  '0x910435b01e6Aa66dE22769062998F6AE98566f23',
  '0x573b009b2dE9A95531f82DA10BB0D793050329d2'
]
```

These accounts use the Ethereum standard format starting with `0x`, but we can simply switch `0x` for `xdc`. By default, the deployment account is the first account from the list above: `xdcA4e66f4Cc17752f331eaC6A20C00756156719519`.

With this account in hand, you can head to the [Apothem Faucet](https://faucet.apothem.network/) and claim some TXDC for development purposes:

![Step 02](https://user-images.githubusercontent.com/78161484/189952656-eb7793cc-7dee-4307-88fc-7c351a75cec7.png)

## 💵 Creating our first DeFi App

The source code for the DeFi App used in this tutorial is available here: [FarmToken Contract](../../how-to/DeFi/Truffle/FarmToken/contracts/FarmToken.sol) and [MyToken Contract](../../how-to/DeFi/Truffle/FarmToken/contracts/MyToken.sol).

### 💵 Create and Deploy XRC20 Token

Before creating FarmToken contract, you should first create a new XRC20 token. This is going to be a quick guide, but if you want an in-depth understanding of the `XRC20` standard, please visit the following tutorials:

* [Create XRC20 token using Hardhat](https://docs.xdc.community/learn/how-to-articles/how-to-create-and-deploy-an-xrc20-token-using-hardhat)
* [Create XRC20 token using Remix](https://docs.xdc.community/learn/how-to-articles/how-to-create-and-deploy-an-xrc20-token-using-remix)
* [Create XRC20 token using Truffle](https://docs.xdc.community/learn/how-to-articles/how-to-create-and-deploy-an-xrc20-token-using-truffle)

We will need to install OpenZeppelin in order to not writing all code by ourself

```
yarn add @openzeppelin/contracts
```

or using `npm`

```
npm install @openzeppelin/contracts
```

Now create a file called `MyToken.sol` in `contracts` folder with following content:

```solidity
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

contract XRC20Token is ERC20 {
    constructor(string memory name, string memory symbol) public ERC20(name, symbol){
        _mint(msg.sender, 1000e18);
    }
}
```

### 💵 Create FarmToken contract

Create another smart contract called `FarmToken.sol`.

```solidity
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC20/IERC20.sol";
import "@openzeppelin/contracts/utils/Address.sol";
import "@openzeppelin/contracts/utils/math/SafeMath.sol";
import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";
import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

contract FarmToken is ERC20 {
    using Address for address;
    using SafeMath for uint256; // As of Solidity v0.8.0, mathematical operations can be done safely without the need for SafeMath
    using SafeERC20 for IERC20;

    IERC20 public token;

    constructor(address _token) public ERC20("FarmToken", "FRM") {
        token = IERC20(_token);
    }

    function balance() public view returns (uint256) {
        return token.balanceOf(address(this));
    }

    function deposit(uint256 _amount) public {
        // Amount must be greater than zero
        require(_amount > 0, "amount cannot be 0");

        // Transfer MyToken to smart contract
        token.safeTransferFrom(msg.sender, address(this), _amount);

        // Mint FarmToken to msg sender
        _mint(msg.sender, _amount);
    }

    function withdraw(uint256 _amount) public {
        // Burn FarmTokens from msg sender
        _burn(msg.sender, _amount);

        // Transfer MyTokens from this smart contract to msg sender
        token.safeTransfer(msg.sender, _amount);
    }
}
```

`FarmToken` is a XRC20 token, but with 3 custom commands. `balance` shows how much of `MyToken` our `FarmToken` holds. `deposit` transfers `MyToken` from our account to `FarmToken` and gives us respective amount of `FarmToken`. `withdraw` burns our `FarmToken` and returns back `MyToken` to us. The latter prevents you from having both same amounts of `FarmToken` and `MyToken`, as you can hold only one of them.

### 💵 Compiling and Deploying

We can compile our smart contracts (`FarmToken.sol` and `MyToken.sol`) by running:

```
truffle compile
```

If everything is correctly configured and there are no errors, you should see the following message on your console:

```
Compiling your contracts...
===========================
> Compiling ./contracts/FarmToken.sol
> Compiling ./contracts/MyToken.sol                                                                                    
> Compiling @openzeppelin/contracts/token/ERC20/ERC20.sol
> Compiling @openzeppelin/contracts/token/ERC20/IERC20.sol                                                             
> Compiling @openzeppelin/contracts/token/ERC20/extensions/IERC20Metadata.sol
> Compiling @openzeppelin/contracts/token/ERC20/extensions/draft-IERC20Permit.sol
> Compiling @openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol 
> Compiling @openzeppelin/contracts/utils/Address.sol
> Compiling @openzeppelin/contracts/utils/Context.sol
> Compiling @openzeppelin/contracts/utils/math/SafeMath.sol
...
```

And your folder should look like this:

![folder\_struct](https://user-images.githubusercontent.com/102393474/192887344-772b3e7a-a43a-47b1-a255-3a6f04f11ba5.png)

In order to deploy our newly-compiled contract artifacts to the blockchain, you'll need to create a deployment script into the migrations folder:

```
touch ./migrations/1_defi_migration.js
```

And write the following migration script to the `1_defi_migration.js` file:

```javascript
const XRC20Token = artifacts.require("XRC20Token");
const FarmToken = artifacts.require("FarmToken")

const NAME = "MyToken";
const SYMBOL = "MTK";

module.exports = async function (deployer) {
  // Deploy XRC20Token
  await deployer.deploy(XRC20Token, NAME, SYMBOL);
  const myToken = await XRC20Token.deployed()

  // Deploy FarmToken
  await deployer.deploy(FarmToken, myToken.address)
  const farmToken = await FarmToken.deployed()

  console.log('FarmToken deployed:', farmToken.address)
}
```

Now, you can run it on a ganache local network. First, open the Ganache app and choose the `Quickstart` option to start your local blockchain network.

![ganache\_home](https://user-images.githubusercontent.com/102393474/192887131-53f9bcec-9843-4027-8ec1-8b442c2d2bb7.png)

Next, deploy contracts by running:

```
truffle migrate
```

If everything is fine, you'll have an output like this:

```
Starting migrations...
======================
> Network name:    'ganache'
> Network id:      5777
> Block gas limit: 6721975 (0x6691b7)


1_defi_migration.js
===================

   Deploying 'XRC20Token'
   ----------------------
   > transaction hash:    0x7c05e8cc683f2125a94e76e037191ddc792e4a1fbecb750d7f299e3f000a1b9d
   > Blocks: 0            Seconds: 0
   > contract address:    0xD63Ab469Ebc2acf0332B7d01028d02E3b363f076
   > block number:        1
   > block timestamp:     1664390940
   > account:             0x7EF6aCFd4F0B8B6828Bf25D440Bf46f1Eb28c6A6
   > balance:             99.97646966
   > gas used:            1176517 (0x11f3c5)
   > gas price:           20 gwei
   > value sent:          0 ETH
   > total cost:          0.02353034 ETH


   Deploying 'FarmToken'
   ---------------------
   > transaction hash:    0x3cb3cf3c959ce40cdc79430b9f78edb2d8f44df4eb25ce5cd1e26631e173aa49
   > Blocks: 0            Seconds: 0
   > contract address:    0xDFe652110afBc0f32060De436F24eDD8E856A82d
   > block number:        2
   > block timestamp:     1664390941
   > account:             0x7EF6aCFd4F0B8B6828Bf25D440Bf46f1Eb28c6A6
   > balance:             99.93702634
   > gas used:            1972166 (0x1e17c6)
   > gas price:           20 gwei
   > value sent:          0 ETH
   > total cost:          0.03944332 ETH

FarmToken deployed: 0xDFe652110afBc0f32060De436F24eDD8E856A82d
   > Saving artifacts
   -------------------------------------
   > Total cost:          0.06297366 ETH

Summary
=======
> Total deployments:   2
> Final cost:          0.06297366 ETH
```

This is what you will see from Ganache app. ![ganache\_0](https://user-images.githubusercontent.com/102393474/192887165-4e766c71-5167-4ef7-9e00-2568ece97580.png)

![ganache\_1](https://user-images.githubusercontent.com/102393474/192887171-6ba7b46d-80d4-4d0b-96a5-092cca193a30.png)

### 💵 Testing FarmToken contract

Create a `scripts` folder where we will put our files for testing.

```bash
mkdir scripts
```

Next, you can create your first script to check balance of `FarmToken`. Create file `getMyTokenBalance.js` and paste:

```javascript
const MyToken = artifacts.require("XRC20Token")
const FarmToken = artifacts.require("FarmToken")

module.exports = async function (callback) {
  myToken = await MyToken.deployed()
  farmToken = await FarmToken.deployed()
  balance = await myToken.balanceOf(farmToken.address)
  console.log(web3.utils.fromWei(balance.toString()))
  callback()
}
```

After that, you can run a script to check our `FarmToken` balance: `truffle exec ./scripts/getMyTokenBalance.js`

```
Using network 'ganache'.

0
```

We now need a script to deposit our tokens in the `FarmToken` contract. Create a file `depositMyToken.js`

```javascript
const MyToken = artifacts.require("XRC20Token")
const FarmToken = artifacts.require("FarmToken")

module.exports = async function (callback) {
  const accounts = await new web3.eth.getAccounts()
  const myToken = await MyToken.deployed()
  const farmToken = await FarmToken.deployed()

  // Returns the remaining number of tokens that spender will be allowed to spend on behalf of owner through transferFrom.
  // This is zero by default.
  const allowanceBefore = await myToken.allowance(
    accounts[0],
    farmToken.address
  )
  console.log(
    "Amount of MyToken FarmToken is allowed to transfer on our behalf Before: " +
      allowanceBefore.toString()
  )

  // In order to allow the Smart Contract to transfer to MyToken (ERC-20) on the accounts[0] behalf,
  // we must explicitly allow it.
  // We allow farmToken to transfer x amount of MyToken on our behalf
  await myToken.approve(farmToken.address, web3.utils.toWei("100", "ether"))

  // Validate that the farmToken can now move x amount of MyToken on our behalf
  const allowanceAfter = await myToken.allowance(accounts[0], farmToken.address)
  console.log(
    "Amount of MyToken FarmToken is allowed to transfer on our behalf After: " +
      allowanceAfter.toString()
  )

  // Verify accounts[0] and farmToken balance of MyToken before and after the transfer
  balanceMyTokenBeforeAccounts0 = await myToken.balanceOf(accounts[0])
  balanceMyTokenBeforeFarmToken = await myToken.balanceOf(farmToken.address)
  console.log("*** My Token ***")
  console.log(
    "Balance MyToken Before accounts[0] " +
      web3.utils.fromWei(balanceMyTokenBeforeAccounts0.toString())
  )
  console.log(
    "Balance MyToken Before TokenFarm " +
      web3.utils.fromWei(balanceMyTokenBeforeFarmToken.toString())
  )

  console.log("*** Farm Token ***")
  balanceFarmTokenBeforeAccounts0 = await farmToken.balanceOf(accounts[0])
  balanceFarmTokenBeforeFarmToken = await farmToken.balanceOf(farmToken.address)
  console.log(
    "Balance FarmToken Before accounts[0] " +
      web3.utils.fromWei(balanceFarmTokenBeforeAccounts0.toString())
  )
  console.log(
    "Balance FarmToken Before TokenFarm " +
      web3.utils.fromWei(balanceFarmTokenBeforeFarmToken.toString())
  )
  // Call Deposit function from FarmToken
  console.log("Call Deposit Function")
  await farmToken.deposit(web3.utils.toWei("100", "ether"))
  console.log("*** My Token ***")
  balanceMyTokenAfterAccounts0 = await myToken.balanceOf(accounts[0])
  balanceMyTokenAfterFarmToken = await myToken.balanceOf(farmToken.address)
  console.log(
    "Balance MyToken After accounts[0] " +
      web3.utils.fromWei(balanceMyTokenAfterAccounts0.toString())
  )
  console.log(
    "Balance MyToken After TokenFarm " +
      web3.utils.fromWei(balanceMyTokenAfterFarmToken.toString())
  )

  console.log("*** Farm Token ***")
  balanceFarmTokenAfterAccounts0 = await farmToken.balanceOf(accounts[0])
  balanceFarmTokenAfterFarmToken = await farmToken.balanceOf(farmToken.address)
  console.log(
    "Balance FarmToken After accounts[0] " +
      web3.utils.fromWei(balanceFarmTokenAfterAccounts0.toString())
  )
  console.log(
    "Balance FarmToken After TokenFarm " +
      web3.utils.fromWei(balanceFarmTokenAfterFarmToken.toString())
  )

  // End function
  callback()
}
```

It's time to deposit some tokens.

```
truffle exec ./scripts/depositMyToken.js
```

```
Using network 'ganache'.

Amount of MyToken FarmToken is allowed to transfer on our behalf Before: 0
Amount of MyToken FarmToken is allowed to transfer on our behalf After: 100000000000000000000
*** My Token ***
Balance MyToken Before accounts[0] 1000
Balance MyToken Before TokenFarm 0
*** Farm Token ***
Balance FarmToken Before accounts[0] 0
Balance FarmToken Before TokenFarm 0
Call Deposit Function
*** My Token ***
Balance MyToken After accounts[0] 900
Balance MyToken After TokenFarm 100
*** Farm Token ***
Balance FarmToken After accounts[0] 100
Balance FarmToken After TokenFarm 0
```

Check your balance again using `truffle exec ./scripts/getMyTokenBalance.js`

```
Using network 'ganache'.

100
```

Everything should be working as intended.

Finally, you can check if we can withdraw your tokens by creating a file called `withdrawMyToken.js`:

```javascript
const MyToken = artifacts.require("XRC20Token")
const FarmToken = artifacts.require("FarmToken")

module.exports = async function (callback) {
  const accounts = await new web3.eth.getAccounts()
  const myToken = await MyToken.deployed()
  const farmToken = await FarmToken.deployed()

  // Verify accounts[0] and farmToken balance of MyToken before and after the transfer
  balanceMyTokenBeforeAccounts0 = await myToken.balanceOf(accounts[0])
  balanceMyTokenBeforeFarmToken = await myToken.balanceOf(farmToken.address)
  console.log("*** My Token ***")
  console.log(
    "Balance MyToken Before accounts[0] " +
      web3.utils.fromWei(balanceMyTokenBeforeAccounts0.toString())
  )
  console.log(
    "Balance MyToken Before TokenFarm " +
      web3.utils.fromWei(balanceMyTokenBeforeFarmToken.toString())
  )

  console.log("*** Farm Token ***")
  balanceFarmTokenBeforeAccounts0 = await farmToken.balanceOf(accounts[0])
  balanceFarmTokenBeforeFarmToken = await farmToken.balanceOf(farmToken.address)
  console.log(
    "Balance FarmToken Before accounts[0] " +
      web3.utils.fromWei(balanceFarmTokenBeforeAccounts0.toString())
  )
  console.log(
    "Balance FarmToken Before TokenFarm " +
      web3.utils.fromWei(balanceFarmTokenBeforeFarmToken.toString())
  )

  // Call Deposit function from FarmToken
  console.log("Call Withdraw Function")
  await farmToken.withdraw(web3.utils.toWei("100", "ether"))

  console.log("*** My Token ***")
  balanceMyTokenAfterAccounts0 = await myToken.balanceOf(accounts[0])
  balanceMyTokenAfterFarmToken = await myToken.balanceOf(farmToken.address)
  console.log(
    "Balance MyToken After accounts[0] " +
      web3.utils.fromWei(balanceMyTokenAfterAccounts0.toString())
  )
  console.log(
    "Balance MyToken After TokenFarm " +
      web3.utils.fromWei(balanceMyTokenAfterFarmToken.toString())
  )

  console.log("*** Farm Token ***")
  balanceFarmTokenAfterAccounts0 = await farmToken.balanceOf(accounts[0])
  balanceFarmTokenAfterFarmToken = await farmToken.balanceOf(farmToken.address)
  console.log(
    "Balance FarmToken After accounts[0] " +
      web3.utils.fromWei(balanceFarmTokenAfterAccounts0.toString())
  )
  console.log(
    "Balance FarmToken After TokenFarm " +
      web3.utils.fromWei(balanceFarmTokenAfterFarmToken.toString())
  )

  // End function
  callback()
}
```

Now, you can run it:

```
truffle exec ./scripts/withdrawMyToken.js
```

```
Using network 'ganache'.

*** My Token ***
Balance MyToken Before accounts[0] 900
Balance MyToken Before TokenFarm 100
*** Farm Token ***
Balance FarmToken Before accounts[0] 100
Balance FarmToken Before TokenFarm 0
Call Withdraw Function
*** My Token ***
Balance MyToken After accounts[0] 1000
Balance MyToken After TokenFarm 0
*** Farm Token ***
Balance FarmToken After accounts[0] 0
Balance FarmToken After TokenFarm 0
```

### 💵 Deploying on a live network

Finally, you can deploy everything on a XDC Apothem Testnet.

```
truffle migrate --network apothem
```

If you want to deploy it on XDC mainet, change network to `xinfin`.

```
truffle migrate --network xinfin
```

## 🔍 Veryfing Contracts on the Block Explorer

Once you have successfully deployed your smart contract to the blockchain, you might want to verify your contract on [XinFin Block Explorer](https://explorer.xinfin.network/).

Because our contract consists of multiple files, we first need to flatten our contract. For that, install `truffle-flattener`.

```bash
yarn add truffle-flattener -g
```

Or using `npm`

```bash
npm install truffle-flattener -g
```

Now, flatten your contract:

```bash
truffle-flattener contracts/MyToken.sol > MyToken_flat.sol
```

Next, open `MyToken_flat.sol` and remove every line which starts with `// SPDX-License-Identifier` except the first one. This is important, as block explorer does not accept contracts with mutliple license definition.

Now, lets grab the `MyToken.sol` address from the deploying step: this address is in the Ethereum standard but we can simply swap the `0x` prefix for `xdc` and search for our newly deployed contract on [XinFin Block Explorer](https://explorer.xinfin.network/):

![Verify 01](https://user-images.githubusercontent.com/78161484/190875518-828c0061-71de-42c2-b222-0b8427852d01.png)

Click on the `Verify And Publish` Option.

You will be redirected to the contract verification page where we need to fill out:

* Contract Name: _XRC20Token_
* Compiler: _Check your_ `truffle-config.js` _file for Compiler Version_
* Contract Code: _Just paste everything from your_ `MyToken_flat.sol` _file_

Once everything is filled out, press Submit!

![Verify 02](https://user-images.githubusercontent.com/78161484/190875635-f6d3aa36-47b2-4b09-ad6a-fe6df3fb11f1.png)

If everything was filled out correctly, your contract page on the block explorer should display a new tab called `Contract`:

![Verify 03](https://user-images.githubusercontent.com/78161484/190875780-6223b4b0-fecc-4e79-83bc-c810c5b0351c.png)

Repeat those steps if you want to verify `FarmToken.sol`, except this time `Contract Name` will be `FarmToken` and source code will be from `FarmToken_flat.sol`

***

For more information about Truffle Suite, Please Visit [Truffle Suite Documentation](https://trufflesuite.com/docs/truffle/).\
For more information about XDC Network, Please Visit [XDC Community Documentation on GitBook](https://docs.xdc.community/).\
Resources used during the deployment of the DeFi App can be found at [FarmToken Contract Folder](https://github.com/XDC-Community/docs/tree/main/how-to/DeFi/Truffle/FarmToken/contracts).
