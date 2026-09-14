---
title: 'How to Build Your First App in Alaio Vibecode'
description: 'Step by step: create a Vibecode key, choose the key type, give the AI agent a prompt with the documentation, launch the app in Bitrix24, and check its security before using it on your account.'
---

App development happens in a dialog between the user and an AI agent. Vibecode gives the agent the documentation, the available objects, the key permissions, and the instructions for launching the app.

1. Open [Vibecode](https://vibecode.bitrix24.com/) and sign in through Bitrix24.

2. Create a key with the permissions you need. For example, working with deals requires access to CRM.

   {% note info "" %}

   Copy the key right away — the platform shows it only once. If you close the page without copying it, you will have to create a new key.

   {% endnote %}

3. Open an AI development tool: Codex, Claude Code, Cursor, or another one.

4. Create a new project and paste a prompt with the key, the documentation link `https://vibecode.bitrix24.com/v1/me`, and the task description.

5. Ask the agent to create a server and launch the app on Vibecode if the app needs a backend or event handlers.

6. Open the app in Bitrix24 and check the main scenario.

## Which Keys to Use

Alaio Vibecode has three key types.

- `vibe_api_...` — a personal key for scenarios that act on behalf of a single user. Authorization works like a webhook with the granted permissions, and no session token is needed. This fits scripts, personal dashboards, server-side integrations, and bots on your own Bitrix24.

- `vibe_app_...` — an authorization key for an app with OAuth. Users sign in to the app through Bitrix24 and work with their own permissions. This key is required for the app to be embedded into the Bitrix24 interface — as an item in the left menu, a tab in a CRM item, or a widget. The personal `vibe_api_...` key does not support interface embedding: with it, the app accesses data but is not placed in the Bitrix24 window.

- `vibe_live_...` — a management key for administration. It is not tied to a single Bitrix24 and does not access data such as deals, tasks, and other objects. This key is used to manage API keys, view the list of connected Bitrix24 accounts, and work with platform support requests.

For a first test, you can use a personal key. For an app that a team will use and that opens inside Bitrix24, choose an authorization key with OAuth — in the creation wizard, this is the *Application with authorization* mode. For how to publish such an app, see [Publishing an App in the Bitrix24 Catalog](vibecode-tools.md#catalog).

### Key Access Mode {#access-mode}

Every key has an access mode. It does not depend on the permissions and determines whether the key can modify data.

- `READWRITE` — read and write, the default value for new keys.

- `READONLY` — read only. Write operations respond with the `403 WRITE_BLOCKED_READONLY_KEY` error.

The effective mode comes in the `GET /v1/me` response, in the `accessMode` field, and it is switched in the key card. The mode for new keys is set by the administrator in the Vibecode configurations: this setting does not affect keys that have already been issued.

## Key Security {#security}

Vibecode works with Bitrix24 access through keys. The key determines which data and actions are available to the AI agent or the app. If the app only needs CRM, do not add access permissions for tasks, chats, or other sections.

Give the key only to the AI tool and the project where the app is being built. Do not publish the key in repositories, code samples, screenshots, or open chats. If the key is exposed, create a new key and disable the old one.

Apps are installed and run only within the Bitrix24 the user has access to. In apps with Bitrix24 authorization, every user works with their own permissions.

Black Hole servers help restrict external access to the app backend. Such a server does not need to be exposed like a regular public VPS: access to the server and the app is managed through Vibecode, and the connection goes through a tunnel.

{% note tip "" %}

For how the platform protects data, how to protect your keys and app on your side, and where the responsibility boundary lies, see the [Security](https://vibecode.bitrix24.com/trust) page.

{% endnote %}

## Permissions for Creating Keys and Apps {#rights}

The Vibecode configurations determine who creates keys, apps, servers, bots, and agents. Issuing an inbound webhook and installing an app is additionally restricted by the Bitrix24 security configurations. The checks run in order: first Vibecode, then Bitrix24.

The error code shows which setting needs to be changed: `KEY_CREATION_RESTRICTED` and `APP_CREATION_RESTRICTED` come from Vibecode, while `CONNECTOR_KEY_ISSUE_FORBIDDEN` and `CONNECTOR_APP_INSTALL_FORBIDDEN` come from Bitrix24. If you are not an administrator, submit an access request from the Vibecode dashboard.

{% note tip "" %}

For the configuration modes, the full list of rejection codes, and the order of checks, see the Vibecode documentation, [Permissions for creating keys and apps](https://vibecode.bitrix24.com/docs/access-rights).

{% endnote %}

## Prompt Examples for an AI Agent

Copy the prompt into Codex, Claude Code, Cursor, or another AI tool. Replace `vibe_api_xxx` or `vibe_app_xxx` with your Vibecode key.

For a first test, use the personal `vibe_api_...` key.

```plaintext
Build a script that collects Bitrix24 CRM data every morning and sends a digest to the manager's chat.

The digest includes: all deals in progress, overdue deals, deals with no activity for 3+ days. Summary: total pipeline amount, top 5 largest deals, list of "forgotten" deals, yesterday's conversion.

API key: vibe_api_xxx
Documentation: https://vibecode.bitrix24.com/v1/me
```

For an app that several employees will use, use the `vibe_app_...` authorization key.

```plaintext
Build an app for Bitrix24 with authorization.

The app must show the user their open tasks, overdue tasks, and tasks with no deadline. Add a status filter and a button to refresh the list.

App authorization key: vibe_app_xxx
Documentation: https://vibecode.bitrix24.com/v1/me
```

The AI agent calls `GET /v1/me`, retrieves the capabilities available to the key, and selects the appropriate API methods. If the key does not have enough permissions, the agent reports which permissions must be added.

{% note info "" %}

You can find more scenarios in the [Solutions](https://vibecode.bitrix24.com/solutions) section — these are tasks you can hand to an AI agent together with a key and a link to the Vibecode documentation.

{% endnote %}

For common tasks — a dashboard, an AI bot, a calculator — the Vibecode dashboard has a section with ready-made technical specifications. You pick one and copy the prompt, and the agent downloads the specification with the same key and builds the app according to it. This section is being rolled out gradually, so it may not be available in your dashboard yet.

## What the AI Agent Does Automatically {#agent-auto}

Once it has the key and the task, the AI agent runs the main development cycle without manual integration setup.

1. Calls `GET /v1/me` to retrieve the available objects, the key permissions, and the instructions.

2. Selects the appropriate Alaio Vibecode API methods.

3. Writes the code for the app, the bot, the event handler, or the background job.

4. Creates a Black Hole server through the Vibecode infrastructure instructions if the app needs a backend.

5. Deploys the app and returns a link to check it.

The AI agent can fix errors that occur while performing the task: identify the cause and retry the action with corrected parameters. If the key permissions, the task data, or the tool access is insufficient, the agent reports what must be added or clarified. An error in Alaio Vibecode itself can be sent by the agent to the platform developers as feedback with the problem context.

## Where the App Runs {#where-runs}

The AI agent can launch the app in Bitrix24 or on a server, depending on the task.

- The app needs no server-side code and no event handling — it is hosted as an HTML/JS app in Bitrix24.

- The app has background logic or is a chatbot — it runs on a [Black Hole server](vibecode-tools.md#black-hole).

For an overview of all platform tools, see [Vibecode Tools](vibecode-tools.md).

## What to Check After Building an App

The AI agent prepares the app code, but the result must be reviewed before you use it on your Bitrix24. Check:

- which permissions the key or the app requests,

- which data the app reads and modifies,

- how the app handles API errors,

- where the Vibecode key and the app's other secrets are stored,

- who gets access to the app or the server,

- which Black Hole server the agent created and who can open the app,

- how the app behaves with test data.

If the agent has changed the code, ask it to briefly describe the architecture, the entry points, the API methods used, and the deployment method. This way the developer can verify the result and continue working with the app.

## What's Next

- [Vibecode Tools](vibecode-tools.md) — Black Hole servers, bots, AI models, AI search, storage, and other tools

- [Collaborating on an App](vibecode-collaboration.md) — the server development team, member roles, and handing an app over to another owner
