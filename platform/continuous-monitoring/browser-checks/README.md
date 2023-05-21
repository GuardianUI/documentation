---
description: >-
  This quickstart gives you the necessary info to create browser checks with
  GuardianUI. It’s helpful to have some prior knowledge of working with
  Javascript.
---

# Browser Checks

## What is a browser check?

A browser check is a Node.js script that controls a headless Chromium browser to mimic user behavior. Load a web page, click a link, create a swap or approval transaction – do everything your users might do with their wallet on your app and check if these result in the correct smart contract interactions.

Your browser checks should verify:

* Transactions point to the correct contracts
* Approvals give the correct addresses access to user funds

The combination of automated interactions and assertions leads to confidence that your app works as expected.

GuardianUI's monitoring system uses tests written in [GuardianTest](/platform/guardian-test/), a custom modification to the popular [Playwright](https://github.com/microsoft/playwright) framework, to power your browser checks.&#x20;

For examples of valid browser checks and instructions for how to create them, see [Writing Tests](/platform/guardian-test/getting-started/writing-your-first-e2e-test/) and [Test Examples](/platform/guardian-test/getting-started/test-examples/.

{% hint style="info" %}
Valid GuardianTest scripts are the foundation of a valid browser check. If the script passes, your check passes. If the script fails, your check fails.
{% endhint %}
