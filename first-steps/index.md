# Where to Start

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The Bitrix24 REST API documentation describes API methods, request examples, and integration scenarios for developing applications and external services.

Below is the recommended sequence for studying the documentation, which will help you:

- understand the documentation structure
- master the basic API capabilities
- move on to more complex scenarios

After completing the starting materials, you will be able to access the REST API, make your first request, choose a development approach, and find methods for your scenario.

## How to Choose a Path

Choose a path based on your task:

- if you need to quickly test the REST API, start with [Access to the REST API](#access) and [Your First API Request](#first-call)
- if you need to embed the REST API into your project, study [REST API Configuration](#settings), [Code Examples](#examples), [Bitrix24 SDK](#sdk), and [API Reference](#api-reference)
- if you need to create a solution inside Bitrix24 without publishing it in the Market, go to [Local Integrations](#local-integrations)
- if you need to prepare an application for publication and monetization, study [Mass-Market Applications](#market)
- if you need to repeat a ready-to-use scenario, open [Ready-to-Use Scenarios](#tutorials)
- if you need to work with an AI agent, choose [AI Tools](#ai-tools)

## What to Prepare Before You Start

Before you start, make sure that:

- you have a Bitrix24 account where the REST API is available. If you do not have access, start with [How to Access the REST API](./access-to-rest-api.md)
- an authorization method is selected: an incoming webhook, a local application, or a mass-market application. The settings are described in [Configuring and Using the REST API](../settings/index.md)
- access permissions are configured for the Bitrix24 tools your integration needs. The list of [available scopes](../api-reference/scopes/permissions.md) depends on the methods the application will call
- webhook secrets, tokens, and application keys are not exposed in public code or logs
- the integration accounts for [REST API limits](../settings/performance/limits.md) on the number of requests

{% note tip "" %}

To understand application architecture, authorization principles, working with the REST API, and environment structure, study the [Bitrix24 App Development Course](https://helpdesk.bitrix24.com/courses/index.php?COURSE_ID=268).

{% endnote %}

## Access to the REST API {#access}

The [How to Access the REST API](./access-to-rest-api.md) section describes how to activate a trial period and gain access to all Bitrix24 REST API methods and capabilities. It also contains information regarding NFR keys for partner developments.

## AI Tools {#ai-tools}

The [AI Tools](../ai-tools/mcp.md) section is dedicated to development with AI agents. 

## REST API Configuration {#settings}

The [Configuring and Using the REST API](../settings/index.md) section covers parameters that affect integration performance:

- authorization domains
- access permissions
- method call specifics
- environment configuration
- request rate limits

The section also includes the article [Configuring Access: Cloud and On-Premise Versions](../settings/cloud-and-on-premise/network-access.md), which describes how to configure network access for incoming and outgoing requests and how to add IP addresses to the allowlist.

## Your First API Request {#first-call}

The [How to Make Your First API Request](./first-rest-api-call.md) section explains how to create an incoming webhook and perform your first REST API method call. This allows you to verify the API functionality and the correctness of your settings.

## Code Examples {#examples}

The [How to Use Examples in the Documentation](./how-to-use-examples.md) section describes how the "Code Examples" block is structured on method pages. The article helps you select a tab for your environment and authorization method, substitute values for placeholders such as `**put_your_webhook_here**`, and add the connection code. It also includes a checklist of reasons why a copied example might not work.

## Bitrix24 SDK {#sdk}

The [SDK for Bitrix24 Development](../sdk/index.md) section describes ready-to-use development libraries that accelerate connection and simplify the creation of applications for Bitrix24.

## Local Integrations {#local-integrations}

The [Overview of Local Integration Tools](../local-integrations/index.md) section is dedicated to tools for creating integrations that operate within Bitrix24 and do not require publication in the Market. The section includes examples of working with local webhooks and applications for task automation and data exchange.

## Ready-to-Use Scenarios {#tutorials}

The [Tutorials: Ready-to-Use REST API Scenarios](../tutorials/index.md) section contains step-by-step guides and ready-to-use scenarios for using the Bitrix24 REST API. For example, how to create and configure a cash register handler or how to retrieve a customer address from the CRM. These examples can be used as a foundation for your own solutions.

## Mass-Market Applications {#market}

The [Mass-Market Applications Overview](../market/index.md) section describes the principles of working with mass-market applications from the Bitrix24 Market, as well as opportunities for their monetization and promotion.

## API Reference {#api-reference}

The [API Reference](../api-reference/index.md) section contains descriptions of the Bitrix24 REST API methods and capabilities. This material helps navigate the API and select methods for specific functionality.
