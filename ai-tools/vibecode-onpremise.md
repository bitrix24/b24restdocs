---
title: 'Alaio Vibecode in the Self-Hosted Version: Connecting the vibecodeconnector Module'
description: 'How to connect Alaio Vibecode in the self-hosted version of Bitrix24: install the Vibecode Connector module, link the account to the Vibecode platform, and start building apps with AI agents.'
---

Alaio Vibecode is available not only in the cloud but also in the self-hosted version of Bitrix24. To connect the platform, install the Vibecode Connector module — technical code `vibecodeconnector` — on your account and connect it to the Vibecode platform.

{% note warning "" %}

The Vibecode Connector module must be installed on one Bitrix24 installation only. If the same license key is used on several Bitrix24 installations, for example, a production one and a test one, Vibecode can be connected on only one of them.

{% endnote %}

Step 1. On the side of the Bitrix24 administrator:

1. Open the administrative section of your Bitrix24.

2. Install the free Vibecode Connector module — technical code `vibecodeconnector` — if it is not installed yet.

3. Open the module settings.

4. Click *Connect to Vibecode*.

5. Wait for the activation to finish: the module registers the Bitrix24 with the Vibecode platform.

Step 2. On the side of the Vibecode user:

1. Open Vibecode and go to the Bitrix24 account selection.

2. Click *Connect self-hosted Bitrix24*.

3. Enter the address of your Bitrix24, for example, `mybox.example.com`, and click *Connect*.

4. Wait for the registration check to finish.

5. Confirm the connection in the browser or in the desktop app.

6. In the chat with the bot, confirm that the request is yours.

After that, Vibecode issues a personal developer key and opens the dashboard for this Bitrix24.

{% note tip "User Documentation" %}

-  [Connecting Alaio Vibecode in the self-hosted version](https://helpdesk.bitrix24.com/open/25980703/)

{% endnote %}

Once connected, working with Alaio Vibecode in the self-hosted version is no different from the cloud: you create keys, give them to an AI agent, and build apps following the same steps.

What's next:

- [How to Build Your First App](vibecode-create-app.md) — creating a key, a prompt for the AI agent, and result verification

- [Vibecode Tools](vibecode-tools.md) — Black Hole servers, bots, AI models, AI search, and other tools

- [Collaborating on an App](vibecode-collaboration.md) — the server development team, member roles, and handing an app over to another owner

- [Alaio Vibecode](vibecode.md) — platform overview
