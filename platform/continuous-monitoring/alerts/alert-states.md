# Alert States

Alert notifications being sent depends on a few factors:&#x20;

1. The alert state of the check (e.g. “passing” or “failing”).
2. The transition between these states.
3. Your notification preferences per alert channel.

## Alert rules

The following table shows all alert states and their transitions.&#x20;

{% hint style="info" %}
Note: The <mark style="color:green;">recovered</mark> condition is met if the prior test result was a <mark style="color:red;">failure</mark> and the most recent test result is a success.&#x20;
{% endhint %}

| **Transition** | **Notification** | **Error**             | **Severity** | **Suggestion for User**                                                                                          |
| -------------- | ---------------- | --------------------- | ------------ | ---------------------------------------------------------------------------------------------------------------- |
| ✅ –> ✅         | None             | N/A                   | N/A          | All clear                                                                                                        |
| ✅ –> ❌         | Failure          | Expected: “0x”        | High         | Investigate immediately. Frontend not creating expected smart contract interactions.                             |
| ✅ –> ❌         | Failure          | Test Timeout          | Medium       | Check test implementation if the error does not resolve itself in the next run                                   |
| ✅ –> ❌         | Failure          | Strict mode violation | Low          | Check test implementation as it may no longer work                                                               |
| ✅ –> ❌         | Failure          | Other                 | Medium       | <p><br></p>                                                                                                      |
| ❌️ –> ❌️       | Failure          |                       |              |                                                                                                                  |
| ❌️ –> ✅        | Recovery         | Expected: “0x”        | High         | Check DNS and deployments if no changes were made                                                                |
| ❌️ –> ✅        | Recovery         | Test Timeout          | Medium       | Test likely failed due to a temporary issue with the GuardianUI test runner. We apologize for the inconvenience. |
| ❌️ –> ✅        | Recovery         | Strict mode violation | Low          | <p><br></p>                                                                                                      |
| ❌️ –> ✅        | Recovery         | Other                 | Medium       | We will provide the error of the prior run that was resolved                                                     |



✅ = passing

❌ = failing
