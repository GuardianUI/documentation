# Running Tests

### Command Line

To run tests, first add the following to your app's `package.json` scripts section:

```json
"test": "yarn bundle && npx playwright test --project=chromium --headed"
```

This bundles the wallet provider object through webpack, and runs all your tests. To then execute this script run either of the following in the command line:

```bash
npm run test
```

or

```bash
yarn test
```
