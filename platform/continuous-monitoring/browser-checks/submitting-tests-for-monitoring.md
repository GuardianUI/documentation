---
description: >-
  In this section, you will learn how to integrate your GuardianTest tests with
  the GuardianUI continuous monitoring system.
---

# Submitting Tests for Monitoring

## Overview

There are three ways for you to create tests that can be integrated with our monitoring system:

1. You write the tests locally or in github using GuardianTest
2. We write and submit your tests for you. _Please contact us using_ [_this form_](https://airtable.com/shrIzrQxUZqBOKabG) _if you're interested in this option._

{% hint style="info" %}
New users will need to contact the GuardianUI team using [this form](https://airtable.com/shr5P7WXayIw9zN1R) to create their GuardianUI workspace and start their 14-day free trial.
{% endhint %}

## How to submit tests to our runners

To submit your tests for continuous monitoring, you will need to follow these steps:

1. [Install the GuardianTest framework](../../readme/getting-started/installation.md)
2. [Write your tests using GuardianTest](../../readme/getting-started/writing-your-first-e2e-test.md)
3. [Run your tests to ensure they pass](../../readme/getting-started/running-tests.md)
4. Log into the [GuardianUI app](https://app.guardianui.com/)
   * If you do not have access to the app, please fill out this form to [receive access](https://airtable.com/shr5P7WXayIw9zN1R) and start your 14-day free trial.&#x20;
5. Click `Write Tests` on the navigation menu
6. Give your test a name using the `Name` input field
7. Select a run frequency using the drop down menu
8. Paste your test code into the in-app code editor
9. Click run to make sure it passes
10. Click submit

{% hint style="info" %}
We are working on github integrations that will allow you to configure your test files to be pulled into our runners from outside our app.&#x20;
{% endhint %}

