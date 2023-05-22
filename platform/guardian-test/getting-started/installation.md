# Installation

## Dependencies

### Node.js

* Check if you already have Node installed by opening a terminal or command prompt instance and executing `node -v`. You should see a version number in the return.
* If you do not have Node already installed, go to [Node.js](https://nodejs.org/en/download) and select the version associated with your operating system.

### Foundry

Check if you already have [Anvil](https://github.com/foundry-rs/foundry/tree/master/anvil) installed through Foundry by opening a terminal or command prompt instance and executing `anvil -V`. You should see a version number in the return. If you do not, follow one of the guides below.

* **Mac, Linux, and Windows**
  * [Install using Foundryup](https://book.getfoundry.sh/getting-started/installation#using-foundryup)
  * [Building from source](https://book.getfoundry.sh/getting-started/installation#building-from-source)
* **Docker**
  * [Installation for Docker](https://book.getfoundry.sh/getting-started/installation#using-foundry-with-docker)
* **CI**
  * [Installation for CI](https://book.getfoundry.sh/getting-started/installation#installing-for-ci-in-github-action)

### GuardianTest

You can install GuardianTest using either npm or yarn:

#### NPM

`npm install --save-dev @guardianui/test`

#### Yarn

`yarn add --dev @guardianui/test`

### Playwright Browsers

`npx playwright install`

## Configuring the Framework

### Playwright Config

At your repo's top-level directory create a file called `playwright.config.ts`.

If you already are using Playwright and already have a `playwright.config.ts:`

* Add `/.*gui\.(js|ts|mjs)/` to the `testMatch` entry to make sure Playwright recognizes our tests
  * If you do not have a `testMatch` entry in the config, add one like shown in the example below
  * If you have existing Playwright tests that are either named with the `testName.spec.ts` or `testName.test.ts` naming conventions make the following your `testMatch` entry: `[/.*gui\.(js|ts|mjs)/, /.*(spec|test)\.(js|ts|mjs)/]`
* Set `fullyParallel` to `false`
* Set `workers` to `1`

```typescript
import { devices, PlaywrightTestConfig } from '@playwright/test';

/**
 * See https://playwright.dev/docs/test-configuration.
 */
const config: PlaywrightTestConfig = {
  // globalSetup: require.resolve('./globalSetup'),
  testDir: './tests',
  testMatch: [/.*gui\.(js|ts|mjs)/],
  /* Maximum time one test can run for. */
  timeout: 90 * 1000,
  expect: {
    /**
     * Maximum time expect() should wait for the condition to be met.
     * For example in `await expect(locator).toHaveText();`
     */
    timeout: 5000
  },
  /* Run tests in files in parallel */
  fullyParallel: false,
  /* Fail the build on CI if you accidentally left test.only in the source code. */
  forbidOnly: !!process.env.CI,
  /* Retry on CI only */
  retries: process.env.CI ? 2 : 0,
  /* Opt out of parallel tests on CI. */
  workers: 1,
  /* Reporter to use. See https://playwright.dev/docs/test-reporters */
  reporter: [["html", { open: "never" }]],
  /* Shared settings for all the projects below. See https://playwright.dev/docs/api/class-testoptions. */
  use: {
    // launchOptions: {
    //   slowMo: 1500,
    // },
    userAgent: "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/106.0.0.0 Safari/537.36",
    headless: true,
    /* Maximum time each action such as `click()` can take. Defaults to 0 (no limit). */
    actionTimeout: 0,
    /* Base URL to use in actions like `await page.goto('/')`. */
    // baseURL: 'http://localhost:3000',

    /* Collect trace when retrying the failed test. See https://playwright.dev/docs/trace-viewer */
    trace: 'on-first-retry',
  },

  /* Configure projects for major browsers */
  projects: [
    {
      name: 'chromium',
      use: {
        ...devices['Desktop Chrome'],
        launchOptions: {
          args: ['--disable-web-security'],
        }
      },
    },

    {
      name: 'firefox',
      use: {
        ...devices['Desktop Firefox'],
      },
    },

    {
      name: 'webkit',
      use: {
        ...devices['Desktop Safari'],
      },
    },

    /* Test against mobile viewports. */
    // {
    //   name: 'Mobile Chrome',
    //   use: {
    //     ...devices['Pixel 5'],
    //   },
    // },
    // {
    //   name: 'Mobile Safari',
    //   use: {
    //     ...devices['iPhone 12'],
    //   },
    // },

    /* Test against branded browsers. */
    // {
    //   name: 'Microsoft Edge',
    //   use: {
    //     channel: 'msedge',
    //   },
    // },
    // {
    //   name: 'Google Chrome',
    //   use: {
    //     channel: 'chrome',
    //   },
    // },
  ],

  /* Folder for test artifacts such as screenshots, videos, traces, etc. */
  // outputDir: 'test-results/',

  /* Run your local dev server before starting the tests */
  // webServer: {},
};

export default config;
```

### env

At your repo's top-level directory create another file called `.env` or if you already have one add the following to your existing `.env`. Comment out whichever line you do not use with a `#` at the start.

```bash
# Must fill in one of these API keys, do not need both
GUARDIAN_UI_INFURA_API_KEY=
GUARDIAN_UI_ALCHEMY_API_KEY=
```

### package.json

To be able to run tests, add the following to your app's `package.json` scripts section:

```json
"test:gui": "npx playwright test --project=chromium --headed"
```

### Tests Folder

Create a folder called `tests` to house your GuardianTest tests within.
