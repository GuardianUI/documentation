---
description: Make sure to name your files using the testName.gui.ts naming convention
---

# Writing Tests

## Core Playwright Overview

### Performing Actions

Actions within tests generally boil down to navigation and interaction. For a more thorough overview, head over to the [Playwright documentation](https://playwright.dev/docs/writing-tests#actions).

#### Navigation

Tests begin with navigating to a specific URL.

```typescript
await page.goto("https://guardianui.com");
```

#### Interaction

Performing interactions involves two pieces.

* The locator of the object to interact with (for more information, check out the [Playwright Locators API documentation](https://playwright.dev/docs/locators)).
* The action to take.

By default Playwright waits for an element to be actionable before attempting to perform an action.

```typescript
// Click the button with text "Click Me!"
await page.locator("button:has-text('Click Me!')").click();
```

Playwright has a robust suite of actions built into the Locator API, the most common are the following:

| Action                   | Behavior                                        |
| ------------------------ | ----------------------------------------------- |
| `locator.check()`        | Check an input checkbox                         |
| `locator.uncheck()`      | Uncheck an input checkbox                       |
| `locator.click()`        | Click an element                                |
| `locator.hover()`        | Hover mouse over an element                     |
| `locator.fill()`         | Fill an input form field in one go              |
| `locator.type()`         | Fill an input form field character by character |
| `locator.focus()`        | Focus an element                                |
| `locator.press()`        | Press a single key                              |
| `locator.selectOption()` | Select on option from a drop down               |

### Making General Assertions

Playwright natively includes test assertions by way of an `expect` function. For more information on the available functionality of `expect`, check out the [Playwright Assertions documentation](https://playwright.dev/docs/test-assertions).

There are generic matchers like `toEqual`, `toContain`, `toBeTruthy`, or `toBeFalsy` that perform simple checks on the value or state of a locator.

```typescript
expect(textLocator).toEqual("Hello world!");

expect(navbarLocator).toBeTruthy();
```

There are also many async assertions which wait until the expected condition is met before continuing with the test. The most common are the following:

| Assertion                           | Behavior                                             |
| ----------------------------------- | ---------------------------------------------------- |
| `expect(locator).toBeChecked()`     | Assert that the checkbox is checked                  |
| `expect(locator).toBeVisible()`     | Assert that an element is visible                    |
| `expect(locator).toContainText()`   | Assert that an element contains certain text         |
| `expect(locator).toHaveAttribute()` | Assert that an element has a certain attribute       |
| `expect(locator).toHaveCount()`     | Assert that a list of elements has a certain length  |
| `expect(locator).toHaveText()`      | Assert that an element matches certain text          |
| `expect(locator).toHaveValue()`     | Assert that an input element matches a certain value |
| `expect(page).toHaveTitle()`        | Assert that the page has a certain title             |
| `expect(page).toHaveURL()`          | Assert that the page has a certain URL               |

### Using Hooks

You can use test hooks to define groups of tests, or hooks like `test.beforeAll`, `test.beforeEach`, `test.afterAll`, and `test.afterEach` to perform reused sets of actions before/after each test or as setup/teardown for test suites. For more information visit the [Playwright Test Hooks documentation](https://playwright.dev/docs/api/class-test).

```typescript
// tests/example.spec.ts
import { test } from "@guardianui/test";
import { expect } from "@playwright/test";

test.describe("hooks example", () => {
    test.beforeEach(async ({ page, gui }) => {
        // Initialize Arbitrum fork before each test
        await gui.initializeChain(42161);
    });
    
    test.afterEach(async ({ page, gui }) => {
        // Do something after each test
    });
    
    test("example test", async ({ page, gui }) => {
        await page.goto("https://guardianui.com");
        
        // Set ETH balance to 1 for the test wallet
        await gui.setEthBalance("1000000000000000000");
        
        // Set ARB balance to 1 for the test wallet
        await gui.setBalance("0x912ce59144191c1204e64559fe8253a0e49e6548", "1000000000000000000");
        
        // Give Permit2 an allowance to spend 1 ARB
        await gui.setAllowance("0x912ce59144191c1204e64559fe8253a0e49e6548", "0x000000000022d473030f116ddee9f6b43ac78ba3", "1000000000000000000");
        
        await expect("text=Guardian").toBeTruthy();
     });
});ypoe
```

## Writing a GuardianTest Test

There are eight phases to writing your first GuardianUI end-to-end test:

1. Forked network initialization
2. Navigation to the dApp
3. Wallet state mocking
4. Wallet connection
5. Performing actions within the dApp
6. Transaction submission and verification

### Phase 1: Forked Network Initialization

In order to set network state and perform network interactions without expending real tokens, every test that plans to interact with a live network needs to begin by initializing a forked network. Within this call you can specify the network you wish to fork using its chain ID, and optionally what block number you want to fork at (if you don't specify a block it runs at the latest). We recommend pinning to a block as it increases deterministicness of the tests.

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

###

### Phase 2: Navigation To The dApp

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

###

### Phase 3: Wallet State Mocking

The test wallet injected into the browser during these tests is empty by default. We can provide it with gas tokens, ERC20 tokens, and set allowances in a few quick lines.

<pre class="language-typescript"><code class="lang-typescript">import { test } from "@guardianui/test";

test.describe("Olympus Example Suite", () => {
    test("Should stake OHM to gOHM", async ({ page, gui }) => {
        // Initialize a fork of Ethereum mainnet (chain ID 1)
        await gui.initializeChain(1);
        
        // Navigate to the Olympus dApp
        await page.goto("https://app.olympusdao.finance/#/stake");
        
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

###

### Phase 4: Wallet Connection

The wallet connection flow will vary slightly from dApp to dApp, but usually requires locating a **"Connect Wallet"** button, and then selecting the **"Metamask"** option in an ensuing modal. While our wallet is not actually Metamask, it is surfaced to apps in a way that looks like Metamask to avoid issues where sites may not have an option to select an injected wallet.

To get a sense of what to write in the test, manually go to your live application and identify the visual and textual elements you need to click to go from the not-connected state to the connected state. Use the "inspect element" tool in browsers to help with this.

See examples in our [test examples](https://app.gitbook.com/o/aEzOvP1ODgbzLwqZ2irE/s/6blK04TyOYOkA5ZEQW4b/end-to-end-testing/test-examples) section.

<pre class="language-typescript"><code class="lang-typescript">import { test } from "@guardianui/test";

test.describe("Olympus Example Suite", () => {
    test("Should stake OHM to gOHM", async ({ page, gui }) => {
        // Initialize a fork of Ethereum mainnet (chain ID 1)
        await gui.initializeChain(1);
        
        // Navigate to the Olympus dApp
        await page.goto("https://app.olympusdao.finance/#/stake");
        
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

###

### Phase 5: Performing Actions Within the dApp

This will vary drastically from dApp to dApp, but generally requires identifying elements on the web page based on class or ID selectors and clicking or entering information into them.

To get a sense of what to write, manually go to your live application and identify the visual and textual elements you need to click to achieve the desired user interaction. Use the "inspect element" tool in browsers to help with this.

<pre class="language-typescript"><code class="lang-typescript">import { test } from "@guardianui/test";

test.describe("Olympus Example Suite", () => {
    test("Should stake OHM to gOHM", async ({ page, gui }) => {
        // Initialize a fork of Ethereum mainnet (chain ID 1)
        await gui.initializeChain(1);
        
        // Navigate to the Olympus dApp
        await page.goto("https://app.olympusdao.finance/#/stake");
        
        // Set ETH balance
        await gui.setEthBalance("100000000000000000000000");
        
        // Set OHM balance
        const ohmToken = "0x64aa3364f17a4d01c6f1751fd97c2bd3d7e7f1d5";
        await gui.setAllowance(ohmToken, "0xb63cac384247597756545b500253ff8e607a8020", "1000000000000000000000000");
        
        // Set staking contract's approval to spend OHM
        await gui.setBalance(ohmToken, "1000000000000000000000000");
        
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
</strong>
<strong>        // Click the stake button to open the modals
</strong><strong>        await page.waitForSelector("[data-testid='submit-button']");
</strong><strong>        await page.locator("[data-testid='submit-button']").click();
</strong>
<strong>        // Click the pre-transaction checkbox
</strong><strong>        await page.waitForSelector("[class='PrivateSwitchBase-input css-1m9pwf3']");
</strong><strong>        await page.locator("[class='PrivateSwitchBase-input css-1m9pwf3']").click();
</strong>        
        // Complete the rest of the test
    });
});
</code></pre>

###

### Phase 6: Transaction Submission and Verification

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
</strong>    });
});
</code></pre>
