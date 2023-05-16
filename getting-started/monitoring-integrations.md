---
description: >-
  Use your tests written with GuardianTest to monitor your live app’s critical
  paths. Get alerted immediately when things break or are compromised.
---

# Monitoring Integrations

{% hint style="info" %}
To get started with a **free trial** using GuardianUI's Continuous Monitoring, please fill out [this form](https://airtable.com/shr5P7WXayIw9zN1R).
{% endhint %}

## Monitoring Overview

Tests written with GuardianTest integrate with [GuardianUI](https://www.guardianui.com/)'s continuous frontend monitoring tool, which helps make sure your app continues working as expected post-production.&#x20;

Specifically, our monitoring platform detects whether your live app is creating the expected smart contract interactions for users, such as transactions pointing to the correct contracts and approvals giving the appropriate access to user funds. If our system detects an issue, we will alert your team in real-time with actionable insights. _You can think of GuardianUI's monitoring solution as providing “continuous frontend auditing.”_

GuardianUI continuously monitors your app for issues that might cause your frontend to create the wrong smart contract interactions for your users. This includes middleware and supply chain attacks that could compromise your frontend and alter its contract interactions, such as DNS attacks, Cloudflare attacks, javascript injections, malicious package injections, package name squatting, malicious minifiers, compromised linters, and more. \


> Coinbase Ventures included the GuardianUI monitoring solution twice in their [Web3 Security Stack](https://twitter.com/jonathankingvc/status/1630599229189046276?s=20) overview.

## Web3 Frontend Attack Examples

These are several examples of the types of web3 frontend attacks we aim to help mitigate. In each of the examples, malicious actors were able to compromise vulnerabilities with a project’s frontend and steal money from users.&#x20;

* [BadgerDAO](https://www.coindesk.com/business/2021/12/02/badger-dao-protocol-suffers-10m-exploit/) (Cloudflare attack) - $120M in user funds stolen
* [Sushiswap’s Miso](https://www.coindesk.com/business/2021/09/17/3m-in-ether-stolen-from-sushiswaps-miso-launchpad/) (Supply chain attack) - $3M in user funds stolen
* [Curve Finance](https://cointelegraph.com/news/curve-finance-exploit-experts-dissect-what-went-wrong) (DNS) - $600k in user funds stolen
* [Ribbon Finance](https://twitter.com/ribbonfinance/status/1540250826156871681?lang=en) (DNS) - $500k in user funds stolen
* [Kyber Network](https://decrypt.co/108831/defi-exchange-kyberswap-suffers-265000-frontend-exploit) (Google Tag Manager) - $265k in user funds stolen
* [Celer Protocol](https://cryptopotato.com/an-estimated-128-eth-lost-in-the-celer-protocol-dns-attack/) (DNS) - $250k in user funds stolen
* [Black Wallet](https://www.trendmicro.com/vinfo/es/security/news/cybercrime-and-digital-threats/-attackers-hijack-dns-entry-of-stellar-lumen-wallet-application-blackwallet) (DNS) - $400k in user funds stolen
* [Pancake Swap](https://decrypt.co/61431/pancakeswap-hacked) (DNS) - prompted users to enter seed phrase
* [Cream Finance](https://thedefiant.io/social-tokens-get-rolled-on/) (DNS) - prompted users to enter seed phrase

### &#x20;Learn more and get started

You can learn more about GuardianUI's Continuous Monitoring product by visiting our [website](https://www.guardianui.com/) or reviewing our docs.

To begin your free trial, please fill out [this form](https://airtable.com/shr5P7WXayIw9zN1R).
