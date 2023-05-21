# Key Benefits

## Non-Brittle Wallet Integration

Testing frameworks that work with wallets already exist in the crypto space. There are essentially two categories:

* Those that only work in the very narrow unit test developer environment and cannot operate in a more end-to-end environment, and
* Those that work by injecting a full browser extension wallet into an end-to-end testing framework such as Cypress.

The frameworks that only work in the very narrow unit test developer environment are limited in their utility because they aren't meant for more thorough testing such as end-to-end tests or continuous monitoring.

The frameworks that work by injecting a full browser extension tend to experience various issues with difficult UX and brittleness. Specifically, the framework constantly needs to be updated to chase supporting updates from wallet providers such as MetaMask. Additionally, by attempting to interact with a browser extension in a simulated browser environment such as Cypress or Playwright, these frameworks introduce additional potential points of failure beyond the functionality of the dApp itself because these environments weren't originally intended to support such interactions with browser extensions.

GuardianTest approaches things differently. By creating a wallet provider object that follows the same base interface that almost every major wallet provider utilizes (EIP-1193), injecting that directly into `window.ethereum`, and having it auto-sign when necessary for actions such as sending transactions or fulfilling signatures, GuardianTest achieves generality and avoids brittleness.

## EVM Network Interaction

GuardianTest can be used for testing across several major networks:

* Ethereum
* Arbitrum
* Optimism
* Polygon

It creates a mocked network at the exact time the developer wants to achieve close to production, but also controllable, network state. You're able to interact with the network without incurring real costs from spending mainnet tokens.

## Simple Chain State Mocking

The framework can mock gas token balances (ETH or MATIC), ERC20 balances, or ERC20 allowances extremely easily with just a single line command. It can also mock the storage of any contract albeit with slightly more difficult UX as the developer needs to know the encoded slot to which to set the storage.

### Chain Interaction Validation

Developers can validate a button click in their frontend triggers an interaction with the correct contract or grants ERC20 approvals to the correct contract.

### Flexibility

Alongside all of the above, the framework can be used to do anything [Playwright](https://github.com/microsoft/playwright) would do normally, including validating the navigability of a site, the copy/layout of a site, or even intercept and mock network requests.
