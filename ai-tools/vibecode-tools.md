---
title: 'Alaio Vibecode Tools: Servers, Bots, AI Models, Search, and Storage'
description: 'An overview of Alaio Vibecode tools for building apps: Black Hole servers, chatbots, AI models and AI search, file storage, version history, AI agents, and the dashboard.'
---

Vibecode combines several tools for building and running Bitrix24 apps. For how to start development, see [How to Build Your First App](vibecode-create-app.md).

#|
|| **Task** | **Tool** ||
|| Manage keys, apps, and servers, view the log and expenses | [Dashboard](#dashboard) ||
|| Run an app backend, a database, or a bot | [Black Hole Servers](#black-hole) ||
|| Back up a server and restore it | [Server Backups](#backups) ||
|| Create a Bitrix24 chatbot | [Bots](#bots) ||
|| Receive Bitrix24 events on the app server | [Event Delivery](#events) ||
|| Analyze text, compose replies and summaries | [AI Models](#ai-models) ||
|| Find information on the internet | [AI Search](#ai-search) ||
|| Store app files | [File Storage](#storage) ||
|| Retain code versions and roll back to them | [Version History](#version-history) ||
|| Assign tasks to a ready-made AI agent in a chat | [AI Agents](#ai-agents) ||
|| Make an app available to Bitrix24 employees | [Publishing in the Catalog](#catalog) ||
|#

## Alaio Vibecode Dashboard {#dashboard}

The [dashboard](https://vibecode.bitrix24.com/dashboard) is the control point for keys, apps, servers, and diagnostics. You can use it alongside an AI agent: the agent writes the code and deploys it, while the developer checks the state of the resources in the interface.

The dashboard provides:

- API keys, app authorization keys, and management keys

- the list of apps and installation data

- Black Hole servers and galaxies, their state and parameters

- file storage and app sources

- AI agents

- analytics on requests and app usage

- an action log for users, apps, and API calls

- access, limit, and budget configurations

- feedback for the Alaio Vibecode team

The analytics and the log help you check which requests the app makes, where errors occur, and which events the agent handles.

The app list works as a showcase: alongside your own apps, you see the ones shared with you and the ones common to the entire Bitrix24. The API returns the same data — `GET /v1/applications` with the `scope` parameter, which accepts the values `mine`, `shared`, and `feed`. An individual app card is returned by `GET /v1/applications/{id}`.

In the feedback section, you can manually create a request for the Alaio Vibecode team. An AI agent can also send feedback through the API: it can offer to send it or do so on request. Such feedback appears in the dashboard as a ticket with the error context, the environment, and the reproduction steps.

## Black Hole Servers {#black-hole}

[Black Hole](https://vibecode.bitrix24.com/blackhole) provides cloud Linux servers for apps. You can use such a server when the app needs a backend, a database, an event handler, a cron job, or a bot.

A Black Hole server is not exposed directly to the internet: external ports are closed, the server is invisible from the internet, and access is opened through a secure Vibecode tunnel. Permissions to view the app can be restricted to the owner only, selected employees, a department, any Bitrix24 employee, any authorized user, or public access.

You can create a server manually in the Vibecode interface or ask an AI agent to prepare the deployment.

```plaintext
Deploy the app to Vibecode.
```

The AI agent can select a suitable server for the app scenario on its own.

After it is created, the server appears in the dashboard, in the Black Hole servers section.

If no requests reach a server, it goes to sleep — by default after it has been idle for 60 minutes. A sleeping server is billed at a reduced rate. Idle time is measured based on incoming requests to the app: cron jobs, background work, and outbound requests do not wake the server or keep it awake. For work at fixed times, set a wake-up schedule, for example, before the working day starts. For continuous operation, choose a non-preemptible plan and disable sleep in the server configurations.

Several apps can be hosted on one shared server — a galaxy. Each app runs in its own container, and the cost is calculated per galaxy and does not depend on the number of apps on it. The hosting mode is selected by the Bitrix24 administrator in the dashboard. For how this works, see [Galaxy App](https://vibecode.bitrix24.com/docs/infra/galaxy).

Several employees can work on the code of an app on a server: the owner assembles a development team and assigns roles to its members. For details, see [Collaborating on an App](vibecode-collaboration.md).

## Server Backups {#backups}

Vibecode can create backups of a Black Hole server — a full disk snapshot: the app code, the database, files, and settings. From a backup, you can deploy a new server if you need to restore the app or roll the environment back.

You can take a backup manually in the dashboard or set a schedule — every day, every two or three days, or once a week. Copies are retained even after the server is deleted. There is no separate fee for backups: you pay for the space the copies take up in the storage.

Restoring creates a new server from the selected copy — the original server is not affected.

## Bots {#bots}

The [Vibecode Bot Platform](https://vibecode.bitrix24.com/bot-platform) helps you create Bitrix24 chatbots. A bot can send messages, handle commands, use buttons, and receive events.

A bot can be described in a prompt the same way as a regular app. The AI agent uses the Vibecode documentation and prepares the code for the required scenario: bot registration, sending messages, handling commands, buttons, files, and events.

For servers without a public address, a bot can request new events through the API itself — for this, the bot is registered in the `fetch` event mode. This fits apps that run on Black Hole and do not accept external webhooks. Outbound requests do not delay server sleep, so for constant polling, configure the server for continuous operation as described in the [Black Hole Servers](#black-hole) section.

## Bitrix24 Event Delivery {#events}

An app on a Black Hole server does not need to poll Bitrix24 for new events itself. Vibecode can subscribe the app to Bitrix24 events, for example, task or deal creation, and deliver them to the server reliably: the platform registers the event handler, retries delivery on failures, and wakes the server up if it is asleep.

An AI agent can set up the subscription with no manual work. A subscription is created and listed with the `POST` and `GET` `/v1/infra/servers/{id}/event-subscriptions` methods and deleted with `DELETE /v1/infra/servers/{id}/event-subscriptions/{subId}`. For MCP clients, the `manage_server_events` tool is available.

Event delivery works under two conditions. The server is bound to an app authorization key `vibe_app_`, and the app has completed OAuth authorization in Bitrix24. Bitrix24 itself is on a commercial plan: on a free plan, the event handler is not registered. For more about subscriptions and delivery formats, see the Vibecode documentation, [Event Subscriptions](https://vibecode.bitrix24.com/docs/infra/event-subscriptions).

## AI Models {#ai-models}

[Vibecode AI Models](https://vibecode.bitrix24.com/ai-models) can be connected to apps and bots through an OpenAI-compatible API. This is needed when the app has to analyze text, compose replies, classify requests, or generate summaries.

Bitrix24 models are available right away and require no setup; most of them are free. The main one is `bitrix/bitrixgpt-5.5`: a 262K token context and built-in image processing. Alongside it, there are a version with a reasoning chain, the open GPT-OSS and Gemma models, agent models with a long context, models for audio transcription and image generation, and an embedding model for the vector representation of text through `POST /v1/embeddings`.

The current list of models is returned by `GET /v1/models`: every model comes with its context, capabilities, and price. Check it before making a choice — the set of models and their terms change. A deprecated model keeps working until its shutdown date, and the response contains the `Deprecation` header. After shutdown, the request is served by the successor model, if one is assigned — its name comes in the `X-Model-Replacement` header.

Usage of paid models is billed in the internal currency, Vibe credits. Models from third-party providers appear in the `GET /v1/models` response only if credentials for those providers have been configured.

If a user has a key from OpenAI, Anthropic, Google Gemini, Groq, DeepSeek, or another OpenAI-compatible provider, it can be connected through the BYOK scenario. In that case, the requests go through Vibecode, while the payment stays on the side of the selected provider.

If you do not pass `model` or you specify `model: "auto"`, Vibecode selects the default model. The API is compatible with the OpenAI `chat/completions` format, so it can be connected through the usual SDKs.

## AI Search {#ai-search}

[Vibecode AI Search](https://vibecode.bitrix24.com/ai-search) helps apps and AI agents retrieve data from the internet. An agent can run a search for a user query through `POST /v1/search`, get a response with links to the sources, and use this data in an app, a bot, or a report. For deeper queries, the deep research mode is available through `POST /v1/research`.

AI search fits scenarios that need external context:

- prepare for a call using the latest news about a company

- put together a short comparison of solutions or competitors

- reply to a client in a chatbot taking current information into account

- find brand mentions for the day and send a digest to a chat

Vibecode provides nine search providers. The built-in one is Bitrix24 AI Search: it composes a response, adds links to the sources, and supports response streaming. You can also connect your own keys for Tavily, Brave Search, Exa, You.com, Linkup, Perplexity Sonar, Jina DeepSearch, or Z.AI. If no provider is specified in the request, search first uses your provider key, then a provider key connected for the entire Bitrix24, and only then the built-in Bitrix24 AI Search.

When you create a key in the dashboard, the `vibe:search` permission is preselected. An AI agent can read the search description in the `GET /v1/me` response and connect search with no manual setup. Requests through Bitrix24 AI Search are paid for in the platform currency, Vibe credits. The platform does not bill searches made with your own provider key — you pay the provider directly.

## File Storage {#storage}

[Vibecode Storage](https://vibecode.bitrix24.com/docs/storage) gives an app a place for files: avatars and logos, form attachments, exports and reports, video attachments in deals, and configuration backups. Files are separated by owner: a personal key works with its owner's files, an app key works with the app's shared files, and when acting on behalf of an employee, with that employee's personal files.

Files up to 10 MB are uploaded directly, and larger ones in parts, up to 5 TB. Every file has a visibility setting: private access through a temporary link that is valid for 10 minutes, or a public permanent address. A public address opens without authorization only if anonymous access is enabled for your Bitrix24. It is disabled by default.

An AI agent can connect the storage with no manual setup: the list of operations comes in the `GET /v1/me` response, and for MCP clients the `vibe_storage_*` tools are available. The storage is paid for as you use it, in the internal currency, Vibe credits — for the storage volume, the outbound traffic, and the write operations.

## App Version History {#version-history}

Vibecode retains the [App Source Code](https://vibecode.bitrix24.com/docs/source-storage) on deployment through the platform. Versions are not tied to a specific developer: if the developer or the AI session changes, the new participant downloads the latest version and continues working without losing the code.

Retention happens automatically at the moment of deployment — no separate API call is needed. If a version could not be retained, the deployment still succeeds, and the retention result comes in the deployment response. Versions can be listed, downloaded through a signed link, tagged `manual` for indefinite retention, or rolled back to before a risky change: download the version and deploy it again. Old versions are deleted automatically once a day, but the five latest versions and versions tagged `manual` or `published` are retained.

A Bitrix24 administrator can disable automatic retention in the Vibecode configurations. After that, new versions do not appear on deployment, while previously retained versions remain available.

Source versions protect against overwriting the work of others when a team works on an app: the deployment reports which version it is based on and is rejected if a newer one has appeared in the storage. For how this works, see [Protection Against Overwriting Someone Else's Work](vibecode-collaboration.md#base-version).

For an AI agent, retaining and loading versions is available through the `save_sources` and `load_sources` MCP tools. The state of the feature comes in the `GET /v1/me` response.

## AI Agents {#ai-agents}

[Vibecode AI Agents](https://vibecode.bitrix24.com/ai-agents) are ready-made agents for Bitrix24 that work as bots in chats. You write to an agent as you would to a colleague: find a deal, create a task, prepare a summary, or check data in CRM. By default, an agent replies only to its creator.

Agents run on Hermes, an open-source AI agent. Hermes works with CRM, tasks, files, the calendar, and chats, understands voice messages and attachments, remembers the dialog context, and picks up new skills as it works.

The agent's capabilities depend on the permissions granted: it acts only within the access it is allowed. The agent's actions are retained in the log, so an administrator can check which requests were made.

The agent's own Black Hole server is paid for separately, in the internal currency, Vibe credits. By default, the agent runs on a preemptible server: it is cheaper, but the cloud restarts it roughly once a day. For continuous operation, the always-online mode is enabled at creation time.

Hermes can be launched through Vibecode: the platform creates a Black Hole server, connects an AI model, and sets up a bot in chats. The user specifies the agent name and selects the AI model and the data the agent will have access to.

An agent can be extended with skills and connected to Vibecode tools: AI models, AI search, and the API for working with Bitrix24 data.

## Publishing an App in the Bitrix24 Catalog {#catalog}

A finished app can be published in the Bitrix24 app catalog so that other employees can install it. The app goes through three statuses:

- private — available to you only

- published — visible to all Bitrix24 employees, placements are bound

- unpublished — placements are unbound, and the card stays in the catalog as inactive

Publication is performed in the dashboard. You specify the app name and description, configure the placements in the Bitrix24 interface, and submit the app to the catalog. Placements are configured in one place: a tab in the deal item, a page in the left menu, and other points are selected there as well, with no separate wizards needed.

A separate deployment for publication is not required: the app becomes available once you switch it to the published status. Publication requires:

- OAuth authorization of the app in Bitrix24

- the `placement` permission for the app key and permissions for the relevant section, for example, `crm` for the deal card

- a retained code version, if source retention is enabled in Bitrix24

- a commercial Bitrix24 plan — the exact condition for your Bitrix24 comes in the `GET /v1/me` response

The server app card in the catalog is a separate mechanism, not app publication. It usually appears after a successful deployment through the platform. If there is no card, the `POST /v1/infra/servers/:id/b24-catalog/publish` method creates it. This method does not bind placements.

An AI agent can manage apps and publication with no manual work: through the `create_app`, `update_app`, `publish_app`, `unpublish_app`, and `manage_placements` MCP tools, or through the `/v1/apps` and `/v1/placements` methods.

## What's Next

- [How to Build Your First App](vibecode-create-app.md) — creating a key, a prompt for the AI agent, and checking the result

- [Collaborating on an App](vibecode-collaboration.md) — the server development team, member roles, and handing the app over to another owner

- [Alaio Vibecode](vibecode.md) — platform overview

- [Alaio Vibecode Documentation](https://vibecode.bitrix24.com/docs/quickstart) — keys, API, and a detailed description of the tools
