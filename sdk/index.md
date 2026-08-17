# SDK for Bitrix24 Development

The Bitrix24 SDK consists of ready-made libraries for the REST API. They take care of authorization, assembling HTTP requests, parsing responses, and handling errors, so your code is left with calling the method you need and working with the result.

The SDKs do not change the composition or the behavior of the methods: the complete reference of methods, parameters, and events is in the [REST API Reference](../api-reference/index.md) section. Three things determine the choice of an SDK — the language of your project, where the code runs, and the authorization method.

## How to Choose an SDK {#all-sdk}

#|
|| **SDK** | **Language and Environment** | **Authorization** | **When to Choose** ||
|| [B24JsSDK](./b24jssdk/index.md) | JavaScript, browser and Node.js | [Webhooks](../local-integrations/local-webhooks.md), [OAuth](../settings/oauth/index.md) | An external web application or a backend service on Node.js ||
|| [BX24.js](./bx24-js-sdk/index.md) | JavaScript, the application frame inside Bitrix24 only | [OAuth](../settings/oauth/index.md) | An application embedded in the Bitrix24 interface ||
|| [B24PhpSDK](./b24phpsdk/index.md) | PHP, server | [Webhooks](../local-integrations/local-webhooks.md), [OAuth](../settings/oauth/index.md) | A complex mass-market application for the Marketplace ||
|| [CRest PHP SDK](./crest-php-sdk/index.md) | PHP, server | [Webhooks](../local-integrations/local-webhooks.md), [OAuth](../settings/oauth/index.md) | A quick API check or a simple local integration ||
|| [B24PySDK](./b24pysdk/index.md) | Python, server | [Webhooks](../local-integrations/local-webhooks.md), [OAuth](../settings/oauth/index.md) | An integration or automation in Python, including Django, FastAPI, and Flask ||
|| [B24GoSDK](./b24gosdk/index.md) | Go, server | [Webhooks](../local-integrations/local-webhooks.md), [OAuth](../settings/oauth/index.md) | A background service in Go with large exports and resilience to connection drops ||
|#

## Key Considerations

- A webhook suits an integration within a single Bitrix24, while OAuth suits an application installed in different Bitrix24 accounts. The details are in the [Webhooks](../local-integrations/local-webhooks.md) and [OAuth 2.0 Protocol](../settings/oauth/index.md) sections
- A webhook key and OAuth tokens grant access to Bitrix24 data. Do not place them in client-side code and do not publish them in a repository
- Access to data is limited by the permissions of the user on whose behalf the webhook or the application operates, and by the list of scopes. The scope values are listed in the [Permissions](../api-reference/scopes/permissions.md) reference
- An SDK does not lift the platform restrictions. The intensity and the resource consumption of requests are described on the [REST API Limits](../limits.md) page

## JavaScript SDK

[B24JsSDK](./b24jssdk/index.md) is a universal JavaScript library for browsers and Node.js. It supports modern language features such as async/await and automatic data type conversion.

Use B24JsSDK if you:

- develop an external web application or a backend service on Node.js
- need [batch](../settings/how-to-call-rest-api/batch.md) requests, typical Bitrix24 objects, and logging
- need flexible authorization: a webhook for a single Bitrix24 or OAuth for a mass-market application

[BX24.js](./bx24-js-sdk/index.md) is a library for applications embedded within the Bitrix24 interface. The code runs in the application frame, so the library receives the authorization data from Bitrix24 itself and substitutes it into requests.

Use BX24.js if you:

- have an application that operates within the Bitrix24 interface
- need access to Bitrix24 methods with ready-made authorization, without a server of your own
- need system selection dialogs, application window management, and configuration storage on the Bitrix24 side

## PHP SDK

[B24PhpSDK](./b24phpsdk/index.md) is a full-fledged PHP client with strict typing, autocompletion, and support for generators for efficient handling of large data sets.

Use B24PhpSDK if you:

- create a complex mass-market application for the Marketplace
- need IDE autocompletion, strict typing, and events for token handling
- need efficient handling of large data sets through PHP generators

[CRest PHP SDK](./crest-php-sdk/index.md) is a starter kit in PHP. It comes with a ready-made `CRest` class.

Use CRest PHP SDK if you:

- need to quickly check how a method works or to assemble a simple integration
- develop a local application for internal tasks
- need a minimal connection setup

## Python SDK

[B24PySDK](./b24pysdk/index.md) is the official Python SDK for the Bitrix24 REST API. It provides a Python interface to Bitrix24 methods, checks types and parameters of requests before sending, returns data in native Python data structures, and handles errors uniformly.

The SDK includes integrations for Django, FastAPI, and Flask: they help validate the data that Bitrix24 sends when opening an application, handling events, and working with business processes.

Use B24PySDK if you:

- develop an application, an integration, or an automation in Python
- need to work with Bitrix24 methods without manually assembling HTTP requests
- need IDE autocompletion, request parameter validation, and predictable response structures
- have a backend application that needs to reliably handle authorization, events, and errors

## Go SDK

[B24GoSDK](./b24gosdk/index.md) is the official Go SDK for the Bitrix24 REST API. It handles authorization, retries on failures, list traversal, and response parsing, while a method is called by its name. There are deliberately no wrappers for individual methods: a method that Bitrix24 releases today is available in the SDK on the same day.

Use B24GoSDK if you:

- develop an integration, an application, or a background service in Go
- need [batch requests](../settings/how-to-call-rest-api/batch.md) and the export of large lists
- need predictable behavior on connection drops and rate limits
- need code without external dependencies

## Getting Started

1. Choose an SDK using the [How to Choose an SDK](#all-sdk) table and open its page — it describes the installation and the first call
2. Set up authorization: a [webhook](../local-integrations/local-webhooks.md) for an integration within a single Bitrix24, or an [application with OAuth](../settings/oauth/index.md) for installation in different Bitrix24 accounts
3. Find the method you need in the [REST API Reference](../api-reference/index.md) section and call it with the tools of the chosen SDK

## Additional Tools

[MCP](../ai-tools/mcp.md) is an auxiliary service for AI assistants in the development environment. MCP provides AI assistants with structured data and specifications for REST API methods, helps generate correct code, and speeds up application creation.

[B24 AI Starter](https://github.com/bitrix24/b24-ai-starter) is a starter template for a Bitrix24 application with instructions for AI agents. The template includes a ready-made Nuxt 3 frontend with the UI Kit connected and three backend options to choose from: PHP, Python, or Node.js — with SDKs, authorization through the [OAuth 2.0 protocol](../settings/oauth/index.md), and a Docker environment. The template saves you from building the application structure from scratch: the AI agent writes the business logic following the ready-made instructions from the repository.

[UI Kit](../api-reference/widgets/ui-kit/index.md) is a set of Vue components for creating interfaces in the Bitrix24 brand style. It ensures consistency in visual language and accelerates frontend development.