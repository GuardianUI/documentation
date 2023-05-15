# Writing Tests

GuardianUI tests do a few core things:

* Perform actions on a website
* Mock network states
* Make general assertions about the state of a site
* Make assertions about contract interactions on a site

## Writing Your First Test

Setting up your first test is very simple. Create a `tests` folder and create a file with the following naming convention inside: `file_name.spec.ts`.

```typescript
// tests/example.spec.ts
import { test } from "@guardianui/test";
import { expect } from "@playwright/test";

test("example test", async ({ page, gui }) => {
    await page.goto("https://guardianui.com");
    
    await expect("text=Guardian").toBeTruthy();
});
```



## Initializing Chains and Wallets

Every test automatically starts with an empty injected wallet connected to Ethereum mainnet. There is a simple command to initiate a fork to begin mocking state and performing interactions. In the event you are testing on a chain other than Ethereum mainnet, you also have to instruct the wallet object of which chain ID to update itself to AFTER the `page.goto` command.

```typescript
// tests/example.spec.ts
import { test } from "@guardianui/test";
import { expect } from "@playwright/test";

test("example test", async ({ page, gui }) => {
    // Initialize a fork of Arbitrum mainnet (chain ID 42161)
    await gui.initializeChain(42161);

    await page.goto("https://guardianui.com");
    
    // Tell the wallet to update to Arbitrum mainnet
    await gui.initializeWallet(42161);
    
    await expect("text=Guardian").toBeTruthy();
});
```



## Mocking Wallet State

The test wallet starts empty. You can easily give it gas token balance (ETH, MATIC, etc), ERC20 balances, and even set allowances for contracts to spend ERC20 tokens in a few simple commands

```typescript
// tests/example.spec.ts
import { test } from "@guardianui/test";
import { expect } from "@playwright/test";

test("example test", async ({ page, gui }) => {
    await gui.initializeChain(42161);

    await page.goto("https://guardianui.com");
    
    await gui.initializeWallet(1);
    
    // Set ETH balance to 1 for the test wallet
    await gui.setEthBalance("1000000000000000000");
    
    // Set ARB balance to 1 for the test wallet
    await gui.setBalance("0x912ce59144191c1204e64559fe8253a0e49e6548", "1000000000000000000");
    
    // Give Permit2 an allowance to spend 1 ARB
    await gui.setAllowance("0x912ce59144191c1204e64559fe8253a0e49e6548", "0x000000000022d473030f116ddee9f6b43ac78ba3", "1000000000000000000");
    
    await expect("text=Guardian").toBeTruthy();
});
```



## Performing Actions

Actions within tests generally consist of navigation and interaction. For a more thorough overview, please review the [Playwright documentation](https://playwright.dev/docs/writing-tests#actions).

### Navigation

Tests begin with navigating to a specific URL.

```typescript
await page.goto("https://guardianui.com");
```

### Interaction

Performing interactions involves two pieces. 
* The locator of the object to interact with (for more information, please see [Playwright Locators API documentation](https://playwright.dev/docs/locators))
* the action to take. 

By default, Playwright waits for an element to be actionable before attempting to perform an action.

```typescript
// Click the button with text "Click Me!"
await page.locator("button:has-text('Click Me!')").click();
```

Playwright has a robust suite of actions built into the Locator API. The most common are the following:

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
| `locator.selectOption()` | Select an option from a drop down               |



## Making General Assertions

Playwright natively includes test assertions by way of an `expect` function. For more information on the available functionality of `expect`, please see the [Playwright Assertions documentation](https://playwright.dev/docs/test-assertions).

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
| `expect(locator).toHaveText()`      | Assert than an element matches certain text          |
| `expect(locator).toHaveValue()`     | Assert that an input element matches a certain value |
| `expect(page).toHaveTitle()`        | Assert that the page has a certain title             |
| `expect(page).toHaveURL()`          | Assert that the page has a certain URL               |



## Making Contract Interaction Assertions

GuardianTest allows you to make assertions around what contract interactions occur when you click a button. The command used for this handles both the clicking of the button and the verification of the interaction.

```typescript
// Assert that the stake button triggers an interaction with the Olympus staking contract
await gui.validateContractInteraction("button:has-text('Stake')", "0xb63cac384247597756545b500253ff8e607a8020");
```



## Using Hooks

You can use test hooks to define groups of tests, or hooks such as `test.beforeAll`, `test.beforeEach`, `test.afterAll`, and `test.afterEach` to perform reused sets of actions before/after each test or as setup/teardown for test suites. For more information visit the [Playwright Test Hooks documentation](https://playwright.dev/docs/api/class-test).

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
        // Tear down Arbitrum fork after each test
        await gui.killChain();
    });
    
    test("example test", async ({ page, gui }) => {
        await page.goto("https://guardianui.com");
        
        await gui.initializeWallet(1);
        
        // Set ETH balance to 1 for the test wallet
        await gui.setEthBalance("1000000000000000000");
        
        // Set ARB balance to 1 for the test wallet
        await gui.setBalance("0x912ce59144191c1204e64559fe8253a0e49e6548", "1000000000000000000");
        
        // Give Permit2 an allowance to spend 1 ARB
        await gui.setAllowance("0x912ce59144191c1204e64559fe8253a0e49e6548", "0x000000000022d473030f116ddee9f6b43ac78ba3", "1000000000000000000");
        
        await expect("text=Guardian").toBeTruthy();
     });
});
```



## Ending a Test

To end a test, you need to tear down the Anvil fork so there aren't collisions in future tests trying to spin up new forks on the same port.

```typescript
await gui.killChain();
```
