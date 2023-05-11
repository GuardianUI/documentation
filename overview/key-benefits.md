# Key Benefits

### Non-brittle Wallet Integration

Testing frameworks that work with wallets exist already in the crypto space. There are essentially two categories. Those that only work in the very narrow unit test developer environment and cannot operate in a more end-to-end environment, and those that work by injecting a full browser extension wallet into an end-to-end testing framework like Cypress. Those that only work in the very narrow unit test developer environment are quite limited in their utility for the reason that they just aren't meant for more thorough testing like end-to-end tests or continuous monitoring. Those that work by injecting a full browser extension experience issues with difficult UX and brittleness. Namely they always have to be updating their framework to chase supporting updates from wallet providers like MetaMask. They also add extra failure points by trying to interact with a browser extension in an environment (a simulated browser environment like Cypress or Playwright) that isn't by default meant to interact like that.

By creating a wallet provider object that follows the same base interface that almost every major wallet provider does (EIP-1193), injecting that directly into `window.ethereum`, and having it auto-sign when necessary for actions like sending transactions or fulfilling signatures we achieve generality and avoid brittleness.



### EVM Network Interaction

The framework can be used for testing across several major networks:

* Ethereum
* Arbitrum
* Optimism
* Polygon

It creates a mocked network at the exact time the developer wants so as to achieve close to production, but also controllable, network state and interact with the network without incurring real costs by spending mainnet tokens.



### Simple Chain State Mocking

The framework can mock gas token (ETH, or MATIC) balances, ERC20 balances, or ERC20 allowances extremely easily with just a single line command. It can also mock the storage of any contract albeit with slightly more difficult UX as the developer needs to know the encoded slot to set the storage at.



### Chain Interaction Validation

Developers can validate that a button click in their frontend triggers an interaction with the correct contract, or grants ERC20 approvals to the correct contract.



### Flexibility

Alongside all of the above, the framework can be used to do anything Playwright would do normally, including validating the navigability of a site, the copy/layout of a site, or even intercept and mock network requests.
