# GuardianUI Overview

[GuardianUI](https://www.guardianui.com/) is an end-to-end testing and monitoring platform for web3 apps. We offer a suite of products designed to improve frontend security and UX in the crypto space, including our open source testing framework - [GuardianTest](https://github.com/GuardianUI/guardianui).

## GuardianUI Ecosystem

### [GuardianTest](https://github.com/GuardianUI/guardianui)

The GuardianUI Test framework ("GuardianTest") is an open source E2E testing framework developed to suit the demands of crypto organizations and developers.

It extends the popular [Playwright](https://playwright.dev/) testing framework so developers can easily:

* Engage with local deployments, staging deployments, OR live production
* Perform tests on EVM compatible chains
* Pin tests to any block
* Interact with a site using a wallet
* Mock ERC20 balances and allowances
* Validate target contract addresses from app interactions
* Perform any other actions or validation Playwright offers

### [Continuous Frontend Monitoring](./#continuous-frontend-monitoring)

Our monitoring system lets users run their tests written with the GuardianTest framework in an ongoing manner against their live app deployment. This can serve as a means of early detection for various frontend attacks that may aim to gain illicit approvals or access to users' tokens such as what happened with [BadgerDAO](https://www.coindesk.com/business/2021/12/02/badger-dao-protocol-suffers-10m-exploit/), [Curve](https://cointelegraph.com/news/curve-finance-exploit-experts-dissect-what-went-wrong), [Ribbon](https://twitter.com/ribbonfinance/status/1540250826156871681?lang=en), and many others.

Learn more about our monitoring product and the frontend exploits it can help detect [here](https://www.guardianui.com/).

### [StoryCheck](https://github.com/GuardianUI/storycheck)

StoryCheck is a system designed to drastically reduce the difficulty of writing end-to-end tests and aims to make it a process non-developers can also participate in. At its core, it converts text into automatically executed test code. It does so by taking markdown formatted user stories with steps written in natural language as input. Then it parses the text and executes steps in a virtual web browser (via Playwright) closely emulating the actions of a real user. It uses [RefExp GPT](https://huggingface.co/spaces/GuardianUI/ui-refexp-click) to predict UI element coordinates given a referring expression.

Learn more about StoryCheck and how you can integrate it into your testing workflow [here](https://github.com/GuardianUI/storycheck).

## Mission

Our mission is simple. We aim to make it easy for web3 developers to create reliable, robust, and safe applications for users. We are currently focused on improving E2E testing and monitoring.
