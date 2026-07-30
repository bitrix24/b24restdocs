# Where to Start

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The Bitrix24 REST API documentation describes API methods, request examples, and integration scenarios for developing applications and external services.

Below is the recommended sequence for studying the documentation, which will help you:

- understand the documentation structure,
- master the basic API capabilities,
- move on to more complex scenarios.

{% note tip "" %}

To understand application architecture, authorization principles, working with the REST API, and environment structure, study the [Bitrix24 App Development Course](https://helpdesk.bitrix24.com/courses/index.php?COURSE_ID=268).

{% endnote %}

## Access to the REST API

The [How to Access the REST API](./access-to-rest-api.md) section describes how to activate a trial period and gain access to all Bitrix24 REST API methods and capabilities. It also contains information regarding NFR keys for partner developments.

## AI Tools

The [AI Tools](../ai-tools/mcp.md) section is dedicated to development with AI agents. 

## REST API Configuration

The [Configuring and Using the REST API](../settings/index.md) section covers parameters that affect integration performance:

- authorization domains,
- access permissions,
- method call specifics,
- environment configuration,
- request rate limits.

The section also includes the article [Configuring Access: Cloud and On-Premise Versions](../settings/cloud-and-on-premise/network-access.md), which describes how to configure network access for incoming and outgoing requests and how to add IP addresses to the allowlist.

## Your First API Request

The [How to Make Your First API Request](./first-rest-api-call.md) section explains how to create an incoming webhook and perform your first REST API method call. This allows you to verify the API functionality and the correctness of your settings.

## Code Examples

The [How to Use Examples in the Documentation](./how-to-use-examples.md) section describes how the "Code Examples" block is structured on method pages: which tab to select for your environment and authorization method, what to substitute for placeholders such as `**put_your_webhook_here**`, and what connection code to add to make the example work in your project. It also includes a checklist of reasons why a copied example might not work.

## Bitrix24 SDK

The [SDK for Bitrix24 Development](../sdk/index.md) section describes ready-to-use development libraries that accelerate connection and simplify the creation of applications for Bitrix24.

## Local Integrations

The [Overview of Local Integration Tools](../local-integrations/index.md) section is dedicated to tools for creating integrations that operate within Bitrix24 and do not require publication in the Market. The section includes examples of working with local webhooks and applications for task automation and data exchange.

## Ready-to-Use Scenarios

The [Tutorials: Ready-to-Use REST API Scenarios](../tutorials/index.md) section contains step-by-step guides and ready-to-use scenarios for using the Bitrix24 REST API. For example, how to create and configure a cash register handler or how to retrieve a customer address from the CRM. These examples can be used as a foundation for your own solutions.

## Mass-Market Applications

The [Mass-Market Applications Overview](../market/index.md) section describes the principles of working with mass-market applications from the Bitrix24 Market, as well as opportunities for their monetization and promotion.

## API Reference

The [API Reference](../api-reference/index.md) section contains descriptions of the Bitrix24 REST API methods and capabilities. This material helps navigate the API and select methods for specific functionality.
