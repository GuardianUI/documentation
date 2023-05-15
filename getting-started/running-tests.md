# Running Tests

## Command Line

To run tests, first add the following to your app's `package.json` scripts section:

```json
"test:gui": "yarn bundle && npx playwright test --project=chromium --headed"
```

This bundles the wallet provider object through webpack and runs all your tests. 

Then execute the script by running either of the following in the command line:

```bash
npm run test:gui
```

or

```bash
yarn test:gui
```
