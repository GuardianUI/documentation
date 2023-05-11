# GuardianUI Overview

The GuardianUI Test framework is a frontend testing tool built to suit the demands of crypto organizations.

It allows you to:

* Engage with local deployments OR live production or staging deployments
* Select a chain and block to perform tests at
* Interact with a site with a wallet
* Easily mock ERC20 balances and allowances
* Validate target contract addresses from app interactions
* Perform any other actions or validation Playwright offers

###

### Ecosystem

GuardianUI has several products built on top of or adjacent to our core testing framework designed to improve frontend security in the crypto space.

#### Monitoring

Our monitoring system lets users run their tests that have been written with the GuardianUI Test framework in an ongoing manner against their live app deployment. This can serve as a means of early detection for various frontend attacks that may aim to gain illicit approvals or access to users' tokens as happened with BadgerDAO, Curve, Ribbon, and many others

Learn more about our monitoring product and the frontend exploits it can help detect here.

#### StoryCheck

StoryCheck is a system designed to drastically reduce the difficulty of writing end-to-end tests with the aim of making it a process non-developers can participate in. At its core it is an end-to-end natural language user story verification system for crypto apps. It takes markdown formatted user stories with steps written in natural language as input. Then it parses the text and executes steps in a virtual web browser (via Playwright) closely emulating the actions of a real user. It uses [RefExp GPT](https://huggingface.co/spaces/GuardianUI/ui-refexp-click) to predict UI element coordinates given a referring expression.

Learn more about StoryCheck and how you can integrate it into your testing workflow here.

###

### Mission

Our mission is simple. Reduce the risk to users and teams that choose to partake in the crypto space. Improving testing and monitoring tooling is the primary focus at the moment.
