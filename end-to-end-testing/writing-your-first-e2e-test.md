# Writing Your First E2E Test

There are eight phases to writing your first E2E test using GuardianTest:

1. Forked network initialization
2. Navigation to the dApp
3. Wallet initialization
4. Wallet state mocking
5. Wallet connection
6. Performing actions within the dApp
7. Transaction submission and verification
8. Stop the forked network



## Phase 1: Forked Network Initialization

In order to set network state and perform network interactions without expending real tokens, every test that plans to interact with a live network needs to begin by initializing a forked network.

<pre class="language-typescript"><code class="lang-typescript">import { test } from "@guardianui/test";

test.describe("Olympus Example Suite", () => {
    test("Should stake OHM to gOHM", async ({ page, gui }) => {
<strong>        // Initialize a fork of Ethereum mainnet (chain ID 1)
</strong><strong>        await gui.initializeChain(1);
</strong>        
        // Complete the rest of the test
    });
});
</code></pre>



## Phase 2: Navigation To The dApp

To navigate to a dApp, provide the framework with the live URL where you wish to perform the test. Use the Playwright `page.goto(url)` asynchronous function to do this.

<pre class="language-typescript"><code class="lang-typescript">import { test } from "@guardianui/test";

test.describe("Olympus Example Suite", () => {
    test("Should stake OHM to gOHM", async ({ page, gui }) => {
        // Initialize a fork of Ethereum mainnet (chain ID 1)
        await gui.initializeChain(1);
        
<strong>        // Navigate to the Olympus dApp
</strong><strong>        await page.goto("https://app.olympusdao.finance/#/stake");
</strong>        
        // Complete the rest of the test
    });
});
</code></pre>



## Phase 3: Wallet Initialization

In order for the test to behave correctly and consistently, you will need to initialize the injected wallet with the chain ID of the network it should be connected to.

<pre class="language-typescript"><code class="lang-typescript">import { test } from "@guardianui/test";

test.describe("Olympus Example Suite", () => {
    test("Should stake OHM to gOHM", async ({ page, gui }) => {
        // Initialize a fork of Ethereum mainnet (chain ID 1)
        await gui.initializeChain(1);
        
        // Navigate to the Olympus dApp
        await page.goto("https://app.olympusdao.finance/#/stake");
        
<strong>        // Initialize wallet to connect to Ethereum mainnet (chain ID 1)
</strong><strong>        await gui.initializeWallet(1);
</strong>        
        // Complete the rest of the test
    });
});
</code></pre>



## Phase 4: Wallet State Mocking

The test wallet injected into the browser during these tests is empty by default. We can provide it with gas tokens, ERC20 tokens, and set allowances in a few quick lines.

<pre class="language-typescript"><code class="lang-typescript">import { test } from "@guardianui/test";

test.describe("Olympus Example Suite", () => {
    test("Should stake OHM to gOHM", async ({ page, gui }) => {
        // Initialize a fork of Ethereum mainnet (chain ID 1)
        await gui.initializeChain(1);
        
        // Navigate to the Olympus dApp
        await page.goto("https://app.olympusdao.finance/#/stake");
        
        // Initialize wallet to connect to Ethereum mainnet (chain ID 1)
        await gui.initializeWallet(1);
        
<strong>        // Set ETH balance
</strong><strong>        await gui.setEthBalance("100000000000000000000000");
</strong><strong>        
</strong><strong>        // Set OHM balance
</strong><strong>        await gui.setAllowance("0x64aa3364f17a4d01c6f1751fd97c2bd3d7e7f1d5", "0xb63cac384247597756545b500253ff8e607a8020", "1000000000000000000000000");
</strong><strong>        
</strong><strong>        // Set staking contract's approval to spend OHM
</strong><strong>        await gui.setBalance("0x64aa3364f17a4d01c6f1751fd97c2bd3d7e7f1d5", "1000000000000000000000000");
</strong>        
        // Complete the rest of the test
    });
});
</code></pre>



## Phase 5: Wallet Connection

The wallet connection flow will vary slightly from dApp to dApp, but usually requires locating a **"Connect Wallet"** button, and then selecting the **"Metamask"** option in an ensuing modal. While our wallet is not actually Metamask, it is surfaced to apps in a way that looks like Metamask to avoid issues where sites may not have an option to select an injected wallet.

To get a sense of what to write in the test, manually go to your live application and identify the visual and textual elements you need to click to go from the not-connected state to the connected state. Use the "inspect element" tool in browsers to help with this. See examples in our [test examples](https://app.gitbook.com/o/aEzOvP1ODgbzLwqZ2irE/s/6blK04TyOYOkA5ZEQW4b/end-to-end-testing/test-examples) section. 

<pre class="language-typescript"><code class="lang-typescript">import { test } from "@guardianui/test";

test.describe("Olympus Example Suite", () => {
    test("Should stake OHM to gOHM", async ({ page, gui }) => {
        // Initialize a fork of Ethereum mainnet (chain ID 1)
        await gui.initializeChain(1);
        
        // Navigate to the Olympus dApp
        await page.goto("https://app.olympusdao.finance/#/stake");
        
        // Initialize wallet to connect to Ethereum mainnet (chain ID 1)
        await gui.initializeWallet(1);
        
        // Set ETH balance
        await gui.setEthBalance("100000000000000000000000");
        
        // Set OHM balance
        await gui.setAllowance("0x64aa3364f17a4d01c6f1751fd97c2bd3d7e7f1d5", "0xb63cac384247597756545b500253ff8e607a8020", "1000000000000000000000000");
        
        // Set staking contract's approval to spend OHM
        await gui.setBalance("0x64aa3364f17a4d01c6f1751fd97c2bd3d7e7f1d5", "1000000000000000000000000");
        
<strong>        // Connect wallet
</strong><strong>        await page.waitForSelector("text=Connect Wallet");
</strong><strong>        await page.locator("text=Connect Wallet").first().click();
</strong><strong>        await page.waitForSelector("text=Connect Wallet");
</strong><strong>        await page.locator("text=Connect Wallet").first().click();
</strong><strong>        await page.locator("text=Metamask").first().click();
</strong><strong>        
</strong><strong>        // This is specific to Olympus. A side tab is opened upon wallet connection
</strong><strong>        // so we click out of it
</strong><strong>        await page.locator("[id='root']").click({ position: {x: 0, y: 0}, force: true });
</strong>        
        // Complete the rest of the test
    });
});
</code></pre>



### Phase 6: Performing Actions Within The dApp

This will vary drastically from dApp to dApp, but generally requires identifying elements on the web page based on class or ID selectors and clicking or entering information into them.

To get a sense of what to write, manually go to your live application and identify the visual and textual elements you need to click to achieve the desired user interaction. Use the "inspect element" tool in browsers to help with this.

<pre class="language-typescript"><code class="lang-typescript">import { test } from "@guardianui/test";

test.describe("Olympus Example Suite", () => {
    test("Should stake OHM to gOHM", async ({ page, gui }) => {
        // Initialize a fork of Ethereum mainnet (chain ID 1)
        await gui.initializeChain(1);
        
        // Navigate to the Olympus dApp
        await page.goto("https://app.olympusdao.finance/#/stake");
        
        // Initialize wallet to connect to Ethereum mainnet (chain ID 1)
        await gui.initializeWallet(1);
        
        // Set ETH balance
        await gui.setEthBalance("100000000000000000000000");
        
        // Set OHM balance
        await gui.setAllowance("0x64aa3364f17a4d01c6f1751fd97c2bd3d7e7f1d5", "0xb63cac384247597756545b500253ff8e607a8020", "1000000000000000000000000");
        
        // Set staking contract's approval to spend OHM
        await gui.setBalance("0x64aa3364f17a4d01c6f1751fd97c2bd3d7e7f1d5", "1000000000000000000000000");
        
        // Connect wallet
        await page.waitForSelector("text=Connect Wallet");
        await page.locator("text=Connect Wallet").first().click();
        await page.waitForSelector("text=Connect Wallet");
        await page.locator("text=Connect Wallet").first().click();
        await page.locator("text=Metamask").first().click();
        
        // This is specific to Olympus. A side tab is opened upon wallet connection
        // so we click out of it
        await page.locator("[id='root']").click({ position: {x: 0, y: 0}, force: true });
        
<strong>        // Performing actions within the dApp
</strong><strong>        // Enter OHM input amount
</strong><strong>        await page.locator("[data-testid='ohm-input']").type("0.1");
</strong><strong>
</strong><strong>        // Click the stake button to open the modals
</strong><strong>        await page.waitForSelector("[data-testid='submit-button']");
</strong><strong>        await page.locator("[data-testid='submit-button']").click();
</strong><strong>
</strong><strong>        // Click the pre-transaction checkbox
</strong><strong>        await page.waitForSelector("[class='PrivateSwitchBase-input css-1m9pwf3']");
</strong><strong>        await page.locator("[class='PrivateSwitchBase-input css-1m9pwf3']").click();
</strong>        
        // Complete the rest of the test
    });
});
</code></pre>



### Phase 7: Transaction Submission and Verification

One of the primary novel behaviors of the GuardianTest framework is its ability to validate information around network transactions following a site interaction such as a button click. Doing this requires two pieces of information:

1. The Playwright locator for the button to interact with.
2. The contract address the button click should trigger a transaction with or an ERC20 approval for.

The GuardianTest framework takes care of the button click itself behind the scenes.

<pre class="language-typescript"><code class="lang-typescript">import { test } from "@guardianui/test";

test.describe("Olympus Example Suite", () => {
    test("Should stake OHM to gOHM", async ({ page, gui }) => {
        // Initialize a fork of Ethereum mainnet (chain ID 1)
        await gui.initializeChain(1);
        
        // Navigate to the Olympus dApp
        await page.goto("https://app.olympusdao.finance/#/stake");
        
        // Initialize wallet to connect to Ethereum mainnet (chain ID 1)
        await gui.initializeWallet(1);
        
        // Set ETH balance
        await gui.setEthBalance("100000000000000000000000");
        
        // Set OHM balance
        await gui.setAllowance("0x64aa3364f17a4d01c6f1751fd97c2bd3d7e7f1d5", "0xb63cac384247597756545b500253ff8e607a8020", "1000000000000000000000000");
        
        // Set staking contract's approval to spend OHM
        await gui.setBalance("0x64aa3364f17a4d01c6f1751fd97c2bd3d7e7f1d5", "1000000000000000000000000");
        
        // Connect wallet
        await page.waitForSelector("text=Connect Wallet");
        await page.locator("text=Connect Wallet").first().click();
        await page.waitForSelector("text=Connect Wallet");
        await page.locator("text=Connect Wallet").first().click();
        await page.locator("text=Metamask").first().click();
        
        // This is specific to Olympus. A side tab is opened upon wallet connection
        // so we click out of it
        await page.locator("[id='root']").click({ position: {x: 0, y: 0}, force: true });
        
        // Performing actions within the dApp
        // Enter OHM input amount
        await page.locator("[data-testid='ohm-input']").type("0.1");

        // Click the stake button to open the modals
        await page.waitForSelector("[data-testid='submit-button']");
        await page.locator("[data-testid='submit-button']").click();

        // Click the pre-transaction checkbox
        await page.waitForSelector("[class='PrivateSwitchBase-input css-1m9pwf3']");
        await page.locator("[class='PrivateSwitchBase-input css-1m9pwf3']").click();
        
<strong>        // Submit stake transaction
</strong><strong>        await gui.validateContractInteraction("[data-testid='submit-modal-button']", "0xb63cac384247597756545b500253ff8e607a8020");
</strong>        
        // Complete the rest of the test
    });
});
</code></pre>



### Phase 8: Stop The Forked Network

GuardianUI tests run in sequence to avoid port collisions when creating and using Anvil forks. However, the fork isn't torn down at the end of a test by default, so to end the test you need to specify that the fork is no longer needed.

<pre class="language-typescript"><code class="lang-typescript">import { test } from "@guardianui/test";

test.describe("Olympus Example Suite", () => {
    test("Should stake OHM to gOHM", async ({ page, gui }) => {
        // Initialize a fork of Ethereum mainnet (chain ID 1)
        await gui.initializeChain(1);
        
        // Navigate to the Olympus dApp
        await page.goto("https://app.olympusdao.finance/#/stake");
        
        // Initialize wallet to connect to Ethereum mainnet (chain ID 1)
        await gui.initializeWallet(1);
        
        // Set ETH balance
        await gui.setEthBalance("100000000000000000000000");
        
        // Set OHM balance
        await gui.setAllowance("0x64aa3364f17a4d01c6f1751fd97c2bd3d7e7f1d5", "0xb63cac384247597756545b500253ff8e607a8020", "1000000000000000000000000");
        
        // Set staking contract's approval to spend OHM
        await gui.setBalance("0x64aa3364f17a4d01c6f1751fd97c2bd3d7e7f1d5", "1000000000000000000000000");
        
        // Connect wallet
        await page.waitForSelector("text=Connect Wallet");
        await page.locator("text=Connect Wallet").first().click();
        await page.waitForSelector("text=Connect Wallet");
        await page.locator("text=Connect Wallet").first().click();
        await page.locator("text=Metamask").first().click();
        
        // This is specific to Olympus. A side tab is opened upon wallet connection
        // so we click out of it
        await page.locator("[id='root']").click({ position: {x: 0, y: 0}, force: true });
        
        // Performing actions within the dApp
        // Enter OHM input amount
        await page.locator("[data-testid='ohm-input']").type("0.1");

        // Click the stake button to open the modals
        await page.waitForSelector("[data-testid='submit-button']");
        await page.locator("[data-testid='submit-button']").click();

        // Click the pre-transaction checkbox
        await page.waitForSelector("[class='PrivateSwitchBase-input css-1m9pwf3']");
        await page.locator("[class='PrivateSwitchBase-input css-1m9pwf3']").click();
        
        // Submit stake transaction
        await gui.validateContractInteraction("[data-testid='submit-modal-button']", "0xb63cac384247597756545b500253ff8e607a8020");
        
        // Stop the forked network
<strong>        await gui.killChain();
</strong>    });
});
</code></pre>
