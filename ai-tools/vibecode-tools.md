---
title: 'Alaio Vibecode Tools: Servers, Bots, AI Models, Search, and Storage'
description: 'An overview of Alaio Vibecode tools for building apps: Black Hole servers, chatbots, AI models and AI search, file storage, version history, AI agents, and the dashboard.'
---

Vibecode combines several tools for building and running Bitrix24 apps. For how to start development, see [How to Build Your First App](vibecode-create-app.md).

## Alaio Vibecode Dashboard

The [dashboard](https://vibecode.bitrix24.com/dashboard) is the control point for keys, apps, servers, and diagnostics. You can use it alongside an AI agent: the agent writes the code and deploys it, while the developer checks the state of the resources in the interface.

The dashboard provides:

- API keys, app authorization keys, and management keys,

- the list of apps and installation data,

- Black Hole servers and galaxies, their state and parameters,

- file storage and app sources,

- AI agents,

- analytics on requests and app usage,

- an action log for users, apps, and API calls,

- access, limit, and budget configurations,

- feedback for the Alaio Vibecode team.

The analytics and the log help you check which requests the app makes, where errors occur, and which events the agent handles.

The app list works as a showcase: alongside your own apps, you see the ones shared with you and the ones common to the entire Bitrix24. The API returns the same data — `GET /v1/applications` with the `scope` parameter, which accepts the values `mine`, `shared`, and `feed`. An individual app card is returned by `GET /v1/applications/{id}`.

In the feedback section, you can manually create a request for the Alaio Vibecode team. An AI agent can also send feedback through the API: it can offer to send it or do so on request. Such feedback appears in the dashboard as a ticket with the error context, the environment, and the reproduction steps.

## Black Hole Servers {#black-hole}

[Black Hole](https://vibecode.bitrix24.com/blackhole) provides cloud Linux servers for apps. You can use such a server when the app needs a backend, a database, an event handler, a cron job, or a bot.

A Black Hole server is not exposed directly to the internet: external ports are closed, the server is invisible from the internet, and access is opened through a secure Vibecode tunnel. Permissions to view the app can be restricted to the owner only, selected employees, a department, any account member, any authorized user, or public access.

You can create a server manually in the Vibecode interface or ask an AI agent to prepare the deployment.

```plaintext
Deploy the app to Vibecode.
```

The AI agent can select a suitable server for the app scenario on its own.

After it is created, the server appears in the dashboard, in the Black Hole servers section.

If a server is not in use, it goes to sleep: while it is idle, compute resources are not billed and only disk storage is paid for. A sleeping server can be woken up on a schedule: set the time when it must wake up automatically, for example, before the working day starts.

Several apps can be hosted on a single server. The cost is calculated per server and does not depend on the number of apps on it. For how this works, see [Galaxy app](https://vibecode.bitrix24.com/docs/infra/galaxy).

Several employees can work on the code of an app on a server: the owner assembles a development team and assigns roles to its members. For details, see [Collaborating on an App](vibecode-collaboration.md).

## Server Backups {#backups}

Vibecode can create backups of a Black Hole server — a full encrypted image of the server disk. From a backup, you can deploy a new server if you need to restore the app or roll the environment back.

Backups can be manual or automatic. A manual backup is created on request in the dashboard. An automatic one runs on a schedule: you set the interval, for example, once a day or once a week, and the number of copies the platform retains. Older copies beyond that number are deleted automatically.

Restoring creates a new server from the selected copy — the original server is not affected.

{% note info "" %}

Server backups are not available on every Bitrix24 — the feature is being enabled gradually. If the dashboard has no server backup management, the feature is not available on your Bitrix24 yet.

{% endnote %}

## Bots

The [Vibecode Bot Platform](https://vibecode.bitrix24.com/bot-platform) helps you create Bitrix24 chatbots. A bot can send messages, handle commands, use buttons, and receive events.

A bot can be described in a prompt the same way as a regular app. The AI agent uses the Vibecode documentation and prepares the code for the required scenario: bot registration, sending messages, handling commands, buttons, files, and events.

For servers without a public address, a bot can request new events through the API itself. This fits apps that run on Black Hole and do not accept external webhooks.

## Bitrix24 Event Delivery

An app on a Black Hole server does not need to poll Bitrix24 for new events itself. Vibecode can subscribe the app to Bitrix24 events, for example, task or deal creation, and deliver them to the server reliably: the platform registers the event handler, retries delivery on failures, and wakes the server up if it is asleep.

An AI agent can set up the subscription with no manual work: the list of operations and the schema come in the `GET /v1/me` response. Subscriptions are managed with the `GET`, `POST`, and `DELETE` `/v1/infra/servers/{id}/event-subscriptions` methods, and for MCP clients with the `manage_server_events` tool.

Event delivery works on commercial Bitrix24 plans where event handler binding is available. For more about subscriptions and delivery formats, see the Vibecode documentation, [Event subscriptions](https://vibecode.bitrix24.com/docs/infra/event-subscriptions).

## AI Models

[Vibecode AI Models](https://vibecode.bitrix24.com/ai-models) can be connected to apps and bots through an OpenAI-compatible API. This is needed when the app has to analyze text, compose replies, classify requests, or generate summaries.

The platform's free models are available right away and require no setup. The main one is `bitrix/bitrixgpt-5.5`: a 262K token context and built-in image processing. Alongside it, there are a version with a reasoning chain, the open GPT-OSS and Gemma models, an agent model with a long context, Whisper for audio transcription, and an embedding model for the vector representation of text through `POST /v1/embeddings`.

The current list of models is returned by `GET /v1/models`: every model comes with its context, capabilities, and price. Check it before making a choice — the set of models and their terms change. When a model gets a successor, it keeps working, and the response contains the `Deprecation` and `X-Model-Replacement` headers with the name of the replacement.

Paid external models are also available through the platform billing, for example, `openai/gpt-4o` and `anthropic/claude-sonnet-4.5` — they are paid for in the internal currency, Vibe credits.

If a user has a key from OpenAI, Anthropic, Google Gemini, Groq, DeepSeek, or another OpenAI-compatible provider, it can be connected through the BYOK scenario. In that case, the requests go through Vibecode, while the payment stays on the side of the selected provider.

If you do not pass `model` or you specify `model: "auto"`, Vibecode selects the default model. The API is compatible with the OpenAI `chat/completions` format, so it can be connected through the usual SDKs.

## AI Search

[Vibecode AI Search](https://vibecode.bitrix24.com/ai-search) helps apps and AI agents retrieve data from the internet. An agent can run a search for a user query through `POST /v1/search`, get a response with links to the sources, and use this data in an app, a bot, or a report. For deeper queries, the deep research mode is available through `POST /v1/research`.

AI search fits scenarios that need external context:

- prepare for a call using the latest news about a company,

- put together a short comparison of solutions or competitors,

- reply to a client in a chatbot taking current information into account,

- find brand mentions for the day and send a digest to a chat.

Vibecode provides nine search providers. Bitrix24 AI Search is used by default: it composes a response, adds links to the sources, and supports response streaming. You can also connect your own keys for Tavily, Brave Search, Exa, You.com, Linkup, Perplexity Sonar, Jina DeepSearch, or Z.AI.

The `vibe:search` permission is added to a key automatically. An AI agent can read the documentation through `GET /v1/me`, understand the request schema, and connect search with no manual setup. AI search requests are paid for in the platform currency, Vibe credits.

## File Storage

[Vibecode storage](https://vibecode.bitrix24.com/docs/storage) gives an app a place for files: avatars and logos, form attachments, exports and reports, video attachments in deals, and configuration backups. The storage is isolated per key — an app works only with its own files and does not see anyone else's.

A file can be uploaded directly, through a presigned link straight from the browser, or in parts for large files up to 5 TB. Every file has a visibility setting: private access through a temporary link that is valid for 10 minutes, or a public permanent address.

An AI agent can connect the storage with no manual setup: the request schema and the list of operations come in the `GET /v1/me` response, and for MCP clients the `vibe_storage_*` tools are available. The storage is paid for as you use it, in the internal currency, Vibe credits — for the storage volume, the outbound traffic, and the write operations.

## App Version History {#version-history}

Vibecode retains the [app source code](https://vibecode.bitrix24.com/docs/source-storage) on every deployment through the platform. Versions are not tied to a specific developer: if the developer or the AI session changes, the new participant downloads the latest version and continues working without losing the code.

Retention happens automatically at the moment of deployment — no separate API call is needed. Versions can be listed, downloaded through a signed link, tagged for indefinite retention, or rolled back to before a risky change: download the version and deploy it again. Old versions are cleaned up on a schedule, while tagged and published versions are retained indefinitely.

Automatic retention can be disabled in the Vibecode configurations so that you do not pay for storing sources. Once it is disabled, new versions do not appear on deployment, so there will be nothing to roll back to.

Source versions protect against overwriting the work of others when a team works on an app: the deployment reports which version it is based on and is rejected if a newer one has appeared in the storage. For how this works, see [Protection Against Overwriting Someone Else's Work](vibecode-collaboration.md#base-version).

For an AI agent, retaining and loading versions is available through the `save_sources` and `load_sources` MCP tools. The state of the feature comes in the `GET /v1/me` response.

## AI Agents

[Vibecode AI Agents](https://vibecode.bitrix24.com/ai-agents) are ready-made agents for Bitrix24 that work as bots in chats. Employees write to an agent as they would to a colleague: find a deal, create a task, prepare a summary, or check data in CRM.

The platform's own agent is Hermes, an open-source AI agent. Hermes works with CRM, tasks, files, the calendar, and chats, understands voice messages and attachments, remembers the dialog context, and picks up new skills as it works.

The agent's capabilities depend on the permissions granted: it acts only within the access it is allowed. The agent's actions are retained in the log, so an administrator can check which requests were made and which data was used.

The Black Hole server that the agent runs on is paid for separately, in the internal currency, Vibe credits. By default, the agent runs on a preemptible server: it is cheaper, but the cloud restarts it roughly once a day. For continuous operation, the always-online mode is enabled at creation time.

Hermes can be launched through Vibecode: the platform creates a Black Hole server, connects an AI model, and sets up a bot in chats. The user selects the agent name and the access permissions.

An agent can be extended with skills and connected to Vibecode tools: AI models, AI search, and the API for working with Bitrix24 data.

## Publishing an App in the Bitrix24 Catalog {#catalog}

A finished app can be published in the Bitrix24 app catalog so that other employees can install it. The app goes through three statuses:

- private — available to you only

- published — visible in the Bitrix24 app catalog

- unpublished — hidden from the catalog

Publication is performed in the dashboard. You specify the app name and description, configure the placements in the Bitrix24 interface, and submit the app to the catalog. Placements are configured in one place: a tab in the deal item, a page in the left menu, and other points are selected there as well, with no separate wizards needed.

A separate deployment for publication is not required: the app becomes available once you switch it to the published status. An app card in the catalog can also be created independently of deployment — by calling `POST /v1/infra/servers/:id/b24-catalog/publish`.

An AI agent can manage apps and publication with no manual work: through the `create_app`, `update_app`, `publish_app`, `unpublish_app`, and `manage_placements` MCP tools, or through the `/v1/apps` and `/v1/placements` methods.
