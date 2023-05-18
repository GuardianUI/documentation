---
description: GUI provides methods related to chain interactions
---

# GUI

## Methods

### initializeChain

Spawns a forked network using Foundry's [Anvil](https://github.com/foundry-rs/foundry/tree/master/anvil) using a specific chain ID and (optionally) a block number. If you do not specify a block number, the test will run at the current block.

#### **Inputs**

* `chainId` - The network chain ID to fork
* `forkBlockNumber` - The block number to fork from (optional)

#### **Usage**

```javascript
// Creates a fork of Ethereum mainnet at block 17110784
await gui.initializeChain(1, 17110784)
```



### killChain

Kills any existing forked networks. Should be used at the end of a test to make sure there are no collisions with tests spawning networks.

#### **Inputs**

* None

#### **Usage**

```javascript
await gui.killChain()
```



### initializeWallet

Updates the wallet provider in the injected wallet to use a specific chain ID.

#### **Inputs**

* `chainId` - The desired network chain ID to update the wallet provider

#### **Usage**

```javascript
// Updates the wallet provider to connect to Arbitrum
await gui.initializeWallet(42161);
```



### setAllowance

Sets the allowance of an address to spend the test wallet's ERC20 tokens to a specific amount.

#### **Inputs**

* `token` - The designated token address for balance allocation (e.g. USDC on Ethereum is [0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48](https://etherscan.io/token/0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48))
* `spenderAddress` - The address to authorize for spending the tokens in the test wallet.
* `amount` - The amount to be approved to be spent by the spender. This needs to have the appropriate number of decimals (i.e. 1 ETH is "1000000000000000000")

#### **Usage**

```javascript
// Gives the Olympus staking contract approval to spend 1 OHM
await gui.setAllowance("0x64aa3364f17a4d01c6f1751fd97c2bd3d7e7f1d5", "0xb63cac384247597756545b500253ff8e607a8020", "1000000000");
```



### setBalance

Sets an ERC20 token's balance for the test wallet.

#### **Inputs**

* `token` - The designated token address for balance allocation.
* `amount` - The amount of tokens to give the test wallet. This needs to have the appropriate number of decimals (i.e. 1 ETH is "1000000000000000000")

#### **Usage**

```javascript
// Gives the test wallet 1 OHM
await gui.setBalance("0x64aa3364f17a4d01c6f1751fd97c2bd3d7e7f1d5", "1000000000");
```

###

### setEthBalance

Sets the test wallet's ETH balance to a specific amount.

#### **Inputs**

* `amount` - The amount of ETH to give to the test wallet. This needs to have the appropriate number of decimals (i.e. 1 ETH is "1000000000000000000")

#### **Usage**

```javascript
// Gives the test wallet 1 ETH
await gui.setEthBalance("1000000000000000000");
```

###

### setContractStorageSlot

Arbitrarily sets a storage slot on a contract to a specific value.

#### **Inputs**

* `contract` - The contract address to set the storage slot on
* `slot` - The keccak256'd storage slot to set
* `value` - The assigned value for the storage slot

#### **Usage**

```javascript
// Sets the first storage slot on the WETH contract to 1
const storageSlot = ethers.utils.solidityKeccak256(["uint256"], 1);
const value = ethers.utils.hexlify(1);
await gui.setContractStorageSlot("0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2", storageSlot, value);
```



### validateContractInteraction

Checks that the `eth_sendTransaction` or `eth_sendRawTransaction` request following a button click is interacting with the correct contract or providing approval to the correct contract.

#### **Inputs**

* `locator` - The Playwright locator string of the button to click
* `targetContract` - The contract address the button should trigger an interaction with

#### **Usage**

```javascript
// Validates that the Olympus "stake" button triggers an interaction with the staking contract
await gui.validateContractInteraction("[data-testid='submit-modal-button']", "0xb63cac384247597756545b500253ff8e607a8020");
```
