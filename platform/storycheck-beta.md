---
description: Create end-to-end tests using natural language
---

# Storycheck (Beta)

## Overview

[StoryCheck](https://github.com/GuardianUI/storycheck#storycheck) is a system designed to drastically reduce the difficulty of writing end-to-end tests and aims to make it a process non-developers can also participate in.&#x20;

At its core, it converts text into automatically executed test code. It does so by taking markdown formatted user stories with steps written in natural language as input. Then it parses the text and executes steps in a virtual web browser (via Playwright) closely emulating the actions of a real user. It uses [RefExp GPT](https://huggingface.co/spaces/GuardianUI/ui-refexp-click) to predict UI element coordinates given a referring expression.

Learn more about StoryCheck and how you can integrate it into your testing workflow [here](https://github.com/GuardianUI/storycheck).

## Demo Video

{% embed url="https://www.youtube.com/watch?v=vpLgTxvpM2w" %}

{% hint style="info" %}
Tests written with Storycheck are not currently configured with our continuous monitoring system.
{% endhint %}
