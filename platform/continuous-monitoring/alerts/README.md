---
description: >-
  Keep your users safe by getting notified immediately if your frontend isn't
  creating the expected smart contract interactions.
---

# Alerts

The Alerts screen gives you the flexibility to decide where you want to be alerted when a check fails and/or recovers. Currently, alerts can automatically be delivered through the following channels:

* Discord
* Email

During each check, we will evaluate the current status of your test (pass/fail) and send you an alert depending on if the [current state](broken-reference) of your test meets our alert conditions.&#x20;

Alerts are sent at the same interval as your check frequency. For example, if your test is scheduled to run every 10 minutes and its status meets our alert conditions, you will receive an alert for that check every 10 minutes until it no longer satisfies our alert conditions.&#x20;
