# Where to Start

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

The REST API is a way to connect Bitrix24 with other programs: a website, an accounting system, a mobile app, or your own service. A program sends Bitrix24 a request to a method — an API command, such as "create a deal". Bitrix24 performs the action and returns a response with the result.

A program that works with Bitrix24 via the REST API is called an integration. It can create deals in the CRM from website inquiries, assign tasks to employees, or reply to customers in chats through a bot.

To get started with the REST API, follow three steps:

1. [Get Access to the REST API](#access)
2. [Make Your First Request](#first-call) and check the Bitrix24 response
3. [Choose the Next Path](#how-to-choose-a-path) for your task

## Access to the REST API {#access}

In Bitrix24 Cloud, the first request on this page requires a Vibe+ plan or a trial. To enable either, see [How to Access the REST API](./access-to-rest-api.md). In the self-hosted version, inbound webhooks do not require a Market subscription. The same article explains how partners developing mass-market applications can get an NFR key, which enables the REST API in a test Bitrix24 environment.

Once you have access, go to the [Your First API Request](#first-call) section.

## Your First API Request {#first-call}

For your first request, create an inbound webhook. It accepts your program's requests at a dedicated URL that contains a secret code. Step-by-step instructions are in the [How to Make Your First API Request](./first-rest-api-call.md) article.

Check the webhook with the [profile](../api-reference/common/users/profile.md) method: it works without parameters and returns the data of the user on whose behalf the request is executed. It does not require selecting a separate [scope](../api-reference/scopes/permissions.md) — a group of methods available to the webhook.

Copy your webhook URL: it already contains the Bitrix24 address, the user ID, and the secret code. Append `profile.json` to the end and open the resulting address in a browser. Address template:

```text
https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/profile.json
```

After the check, delete this address from your browser history: it contains the secret webhook code. Do not publish the code in repositories, on web pages, or in logs.

The `result` field contains the data returned by the method, and the `time` field contains request metadata. See the [profile](../api-reference/common/users/profile.md) method page for the full response and field descriptions. An abbreviated response is shown below:

```json
{
    "result": {
        "ID": "1",
        "NAME": "Klaus",
        "LAST_NAME": "Weber"
    },
    "time": {
        "duration": 0.0401120185852051
    }
}
```

This response means the webhook works. Next, call methods for your task, for example [crm.item.add](../api-reference/crm/universal/crm-item-add.md) to create a deal. This method requires the `crm` scope; the scopes required by other methods are listed on their pages.

If the response contains the `error` and `error_description` fields instead of `result`, the request failed. For example, if the URL contains an error in the secret webhook code or the user ID, Bitrix24 returns:

```json
{
    "error": "INVALID_CREDENTIALS",
    "error_description": "Invalid request credentials"
}
```

In this case, check the user ID and secret code against the webhook URL in Bitrix24. For other errors, see [Error Codes](../error-codes.md).

## How to Choose a Path

After your first request, choose what to do next:

| Task | Where to continue |
| --- | --- |
| Explore the capabilities of the REST API | [What the Bitrix24 REST API Allows](../developing-with-rest-api.md) |
| Embed the REST API into your project | [REST API Configuration](#settings), [Code Examples](#examples), [Bitrix24 SDK](#sdk), and [API Reference](#api-reference) |
| Create a solution for your own Bitrix24 | [Local Integrations](#local-integrations) |
| Prepare an application for publication and monetization | [Mass-Market Applications](#market) |
| Repeat a ready-to-use scenario | [Ready-to-Use Scenarios](#tutorials) |
| Work with an AI agent | [AI Tools](#ai-tools) |

## REST API Configuration {#settings}

The [Configuring and Using the REST API](../settings/index.md) section answers questions that arise when developing an integration:

- how to call methods and pass parameters
- how to authorize via OAuth 2.0
- how to install and configure an application
- how working in the cloud differs from working in the self-hosted version
- how to work under load without hitting the limits

### What to Prepare Before You Start

Choose a [connection method](#local-integrations): an inbound webhook or an application. In its settings, specify the required [scopes](../api-reference/scopes/permissions.md), for example `crm` to work with the CRM. A scope does not extend user permissions: if the user cannot see other employees' deals, an integration acting on their behalf cannot see them either.

The secret webhook code, tokens, and application keys must not end up in public repositories, on web pages, or in logs.

In Bitrix24 Cloud, take the [REST API limits](../settings/performance/limits.md) into account: if you send requests too often, Bitrix24 temporarily rejects them. In the self-hosted version, the load is configured on the server side.

If a self-hosted Bitrix24 or your application runs in a closed corporate network, see the [Required Network Access](../settings/cloud-and-on-premise/network-access.md) article. It describes which requests to allow on the firewall and where to get the IP addresses. Bitrix24 Cloud does not need this configuration.

To understand application architecture, authorization, and environment setup, study the [Bitrix24 App Development Course](https://helpdesk.bitrix24.com/courses/index.php?COURSE_ID=268).

## Code Examples {#examples}

The [How to Use Examples in the Documentation](./how-to-use-examples.md) article describes how the "Code Examples" block is structured on method pages. It helps you select a tab for your environment and authorization method, substitute values for placeholders such as `**put_your_webhook_here**`, and add the connection code. It also covers common errors that can prevent a copied example from working.

## Bitrix24 SDK {#sdk}

The [SDK for Bitrix24 Development](../sdk/index.md) section describes ready-to-use libraries for JavaScript, PHP, Python, and Go. They handle authorization and parse responses, so the integration code is shorter.

## Local Integrations {#local-integrations}

The [Tools for Local Integrations](../local-integrations/index.md) section is dedicated to webhooks and applications for integrations that work with a single Bitrix24 and do not require publication in the Market. It also includes examples of automating routine operations and data exchange.

Choose a connection method:

- an inbound webhook — for an integration that works on behalf of a single user. Requests are executed with the permissions of the user who created the webhook
- an application — for an integration that works on behalf of different users or is embedded in the Bitrix24 interface. An application is installed in Bitrix24, for example to add a tab to a deal card. The installation procedure is described in the [Application Installation Options in Bitrix24](../settings/app-installation/index.md) article

## Ready-to-Use Scenarios {#tutorials}

The [Tutorials: Ready-to-Use REST API Scenarios](../tutorials/index.md) section contains step-by-step guides, for example how to retrieve a customer address from the CRM. These examples can be used as a foundation for your own solutions.

## Mass-Market Applications {#market}

The [Mass-Market Applications Overview](../market/index.md) section describes the principles of working with mass-market applications from the Bitrix24 Market, as well as opportunities for their monetization and promotion.

## AI Tools {#ai-tools}

For more about tools for developing with an AI agent, see the [Alaio Vibecode](../ai-tools/vibecode.md) and [MCP Server](../ai-tools/mcp.md) articles.

## API Reference {#api-reference}

The [API Reference](../api-reference/index.md) section contains descriptions of the Bitrix24 REST API methods and capabilities. This material helps navigate the API and select methods for specific functionality. Both API versions — the previous version and REST 3.0 — are available concurrently. See [Which API Version to Choose](../api-reference/index.md#version) for guidance.
