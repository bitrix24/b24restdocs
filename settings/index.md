# Configuring and Using the REST API

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

This section provides answers to practical questions that arise during integration development:
- how to securely access the REST API,
- how to configure different application types,
- how to optimize load.

To make your first request, study the [First Steps](../first-steps/index.md) section.

To find a specific Bitrix24 REST API method, use the [Method Reference](../api-reference/index.md).

## Configuring REST Calls {#start}

Review materials explaining how to form requests, encode data, and retrieve sequential selections.

- What a basic request looks like and how response formats differ — [How a Request Is Executed](how-to-call-rest-api/general-principles.md).
- How to perform sequential method calls and pass results between requests — [How to Execute a Batch Request](how-to-call-rest-api/batch.md).
- What to do if parameters contain special characters — [Data Encoding](how-to-call-rest-api/data-encoding.md).
- How to correctly retrieve lists and work with the `start` parameter — [List Method Specifics](how-to-call-rest-api/list-methods-pecularities.md).
- Why a request stopped working after a domain change and how to handle redirects — [REST Call Specifics When a Bitrix24 Address Changes](how-to-call-rest-api/change-domen.md).

## Performing Authorization {#auth}

Choose an authorization method — from simple webhooks to the full OAuth 2.0 cycle, and configure automatic token renewal and error handling.

- The difference between webhooks and the OAuth protocol — [Authorization in REST](how-to-call-rest-api/authorization.md).
- How OAuth authorization works step by step — [Full OAuth 2.0 Authorization Protocol](oauth/index.md).
- How to perform authorization directly within the application interface or via an installation event — [Simplified Token Retrieval](oauth/simple-way.md).
- How to avoid losing access overnight and when to refresh `refresh_token` — [Automatic OAuth 2.0 Token Renewal](oauth/auto-renewal.md).
- How to troubleshoot `invalid_client`, `insufficient_scope`, and other errors — [Error Codes](oauth/error-codes.md).

## Installing and Configuring an Application {#app}

Choose the appropriate scenario and follow the instructions for local, mass-market, or configuration solutions.

- Choosing the right application type — [Application Installation Options in Bitrix24](app-installation/index.md).
- How to add a local application — [Local Application Installation Overview](app-installation/local-apps/index.md).
- How to create and install a mass-market solution — [Mass-Market Application Installation Overview](app-installation/mass-market-apps/index.md).
- When to call `installFinish` and what to check before launching — [Completing Application Installation](app-installation/installation-finish.md).
- How to publish ready-made sites, industry CRMs, and Smart scripts — [Installing Site Templates](app-installation/site-templates-installation.md), [Installing Industry CRMs](app-installation/vertical-crm-installation.md), [Installing Solutions with Smart Scripts](app-installation/smart-scripts-installation.md).
- Which data is deleted when an application is removed and how to handle the deletion event — [Deleting Applications](app-uninstallation.md).

## Configure Application Operation in Cloud and Self-Hosted {#box}

Compare the requirements for the cloud and self-hosted versions, configure the network, and extend the API if necessary.

- What to consider for application operation in the self-hosted version — [REST API Usage Differences](cloud-and-on-premise/index.md).
- Why a method might be unavailable — [Module Versioning in Self-Hosted Bitrix24](cloud-and-on-premise/on-premise/versions.md).
- Which domains to open and where to get the IP list for event queues — [Required Network Accesses](cloud-and-on-premise/network-access.md).
- What to do if the corporate network is isolated and the authorization server needs to be replaced — [Application Authorization in an Isolated Self-Hosted Bitrix24](cloud-and-on-premise/on-premise/custom-auth-provider.md).
- How to add custom methods and new scopes — [Adding Custom Methods to the Self-Hosted Bitrix24 REST API](cloud-and-on-premise/on-premise/custom-methods.md).
- Which security requirements to consider during development — [Security Recommendations for REST API Applications](cloud-and-on-premise/security-recommendations.md).

## Implement an Interactive Interface and Instant Events {#push}

Use Push & Pull to react instantly to user actions in the interface.

- Which interactivity options are available — [Interactive Applications](interactivity/index.md).
- How to create and configure your own Push & Pull client — [Custom Push & Pull Client](interactivity/custom-push-and-pull-client.md).
- Which methods to use to retrieve connection parameters and send push events — [Push & Pull](interactivity/push-and-pull/index.md).

## Optimize Performance Under Load {#limits}

Apply recommendations for query optimization, queue management, and limit control.

- How to reduce the number of requests and speed up responses — [General Recommendations](performance/index.md).
- How to export thousands of items and avoid counting the total amount — [How to Retrieve Large Volumes of Data](performance/huge-data.md).
- How to build an incoming queue and protect handlers from spikes — [Incoming Event Queue](performance/queue.md).
- Which restrictions apply to the REST API — [Limits](performance/limits.md).
