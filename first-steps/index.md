# Getting Started

The Bitrix24 REST API documentation describes API methods, request examples, and integration scenarios for developing applications and external services.

Below is a recommended sequence for studying the documentation that will help you:

- understand the structure of the documentation,
- master the basic capabilities of the API,
- move on to more complex scenarios.

{% note tip  "" %}

To understand the architecture of applications, authorization principles, working with the REST API, and the environment structure, study the [application development course for Bitrix24](https://dev.1c-bitrix.com/learning/course/index.php?COURSE_ID=266&INDEX=Y).

{% endnote %}

## Accessing the REST API

The section [How to Access the REST API](./access-to-rest-api.md) describes how to activate the trial period and gain access to all methods and capabilities of the Bitrix24 REST API. It also includes information about NFR keys for partner developments.

## Configuring the REST API

The section [Configuring and Using the REST API](../settings/index.md) is dedicated to parameters that affect integration operations:

- authorization domains,
- access permissions,
- method call specifics,
- environment configuration,
- request limits.

This section also includes the article [Configuring Access: Cloud and On-Premise Versions](../settings/cloud-and-on-premise/network-access.md), which explains how to set up network access for incoming and outgoing requests and how to add IP addresses to the allowed list.

## Making Your First API Request

The section [How to Make Your First API Request](./first-rest-api-call.md) explains how to create an incoming webhook and make your first REST API method call. This allows you to verify the API's functionality and the correctness of your settings.

## Request Examples

The section [How to Use Examples in the Documentation](./how-to-use-examples.md) describes how to work with request examples, which programming languages are used, and how to connect to libraries. It also shows how to adapt examples to your specific task and make real requests to the API.

## Bitrix24 SDK

The section [SDK for Bitrix24 Development](../sdk/index.md) describes ready-made libraries for development and provides information on connecting the MCP service for AI assistants in the development environment.

## Local Integrations

The section [Overview of Tools for Local Integrations](../local-integrations/index.md) is dedicated to tools for creating integrations that work within Bitrix24 and do not require publication in the Marketplace. This section includes examples of working with local webhooks and applications for task automation and data exchange.

## Ready-Made Scenarios

The section [Tutorials: Ready-Made Scenarios for Using the REST API](../tutorials/index.md) contains step-by-step guides and ready-made scenarios for using the Bitrix24 REST API. For example, how to create and configure a cash register handler or how to retrieve a client's address from the CRM. These examples can be used as a foundation for your own solutions.

## Mass-Market Applications

The section [Overview of Mass-Market Applications](../market/index.md) describes the principles of working with mass-market applications from the Bitrix24 Marketplace, as well as opportunities for monetization and promotion.

## API Reference

The section [API Reference](../api-reference/index.md) contains descriptions of methods and capabilities of the Bitrix24 REST API. This material helps you navigate the API and select methods for specific functionalities.

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../sdk/mcp.md) so that the assistant uses the official REST documentation.

{% endnote %}