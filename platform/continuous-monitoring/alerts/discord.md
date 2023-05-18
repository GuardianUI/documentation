# Discord

GuardianUI integrates with [Discord](https://discord.com/) and can deliver alerts directly to your Discord server / channel.

Here's how to set it up:

1. First, create a **Webhook**. Log in to Discord and go to “Server Settings” > “Integrations” and click “Create Webhook”.

<figure><img src="../../../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

2\. Choose a name for your integration like “GuardianUI” and [add this GuardianUI icon](https://drive.google.com/uc?export=download\&id=1KFbb4a8qowbSnJO1VHlRcTtztkQD0gFK). Select which channel in your server you want to receive the alerts. Click the “Copy the Webhook URL” to copy the Webhook URL.&#x20;

<figure><img src="../../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

3\. Log into GuardianUI and navigate to your Alerts dashboard. Click the “Setup” button for Discord.

<figure><img src="../../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

4\. Paste the Webhook URL in the input field.

<figure><img src="../../../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

5\. Click “Save Discord Webhook”

Voila! You successfully integrated GuardianUI with Discord!

<figure><img src="../../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

* **Title** - this will either be Test Failure or Test Recovery depending on the alert’s status. The test name is included under the title. In the example above, Fake Test is the test name.
* **Project** - the GuardianUI workspace associated with the test
* **Type** - the type of error based on our alert rules
* **Severity** - the severity of error based on our alert rules
* **Test** - URL of the individual test page
* **Message -** GuardianUI’s recommendation for how to handle the alert.
