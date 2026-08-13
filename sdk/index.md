# SDK for Bitrix24 Development

The Bitrix24 SDK consists of ready-made libraries for the REST API. They accelerate integration and simplify the creation of applications for Bitrix24.

## JavaScript SDK

[B24JsSDK](./b24jssdk/index.md) is a universal JavaScript library for browsers and Node.js. It supports modern language features such as async/await and automatic data type conversion.

Use B24JsSDK:

- if you need to develop an external web application or a backend service on Node.js,
- if support for batch requests [batch](../settings/how-to-call-rest-api/batch.md), typical Bitrix24 objects, and logging is required,
- if flexible authorization through [OAuth](../settings/oauth/index.md) or [webhooks](../local-integrations/local-webhooks.md) is necessary.

[BX24.js](./bx24-js-sdk/index.md) is a classic SDK for applications embedded within the Bitrix24 interface. Authorization is available only through the [OAuth protocol](../settings/oauth/index.md).

Use BX24.js:

- if the application operates within the Bitrix24 interface,
- if quick access to the REST API with ready-made authorization through [OAuth](../settings/oauth/index.md) is needed,
- if built-in UI tools are required.

## PHP SDK

[B24PhpSDK](./b24phpsdk/index.md) is a full-fledged PHP client with strict typing, autocompletion, and support for generators for efficient handling of large data sets. Authorization is available through [webhooks](../local-integrations/local-webhooks.md) and the [OAuth protocol](../settings/oauth/index.md).

Use B24PhpSDK:

- if you need to create a complex mass-market application for a marketplace,
- if IDE autocompletion, strict typing, and events for token handling are required,
- if efficient handling of large data sets through PHP generators is important.

[CRest PHP SDK](./crest-php-sdk/index.md) is a simple starter kit in PHP. It comes with a ready-made `CRest` class.

Use CRest PHP SDK:

- if you need to quickly test the API or create a simple integration,
- if you plan to develop a local application for internal needs,
- if minimal configuration for working with [webhooks](../local-integrations/local-webhooks.md) or [OAuth](../settings/oauth/index.md) is required.

## Python SDK

[B24PySDK](./b24pysdk/index.md) is the official Python SDK for the Bitrix24 REST API. It provides a convenient Python interface to API methods, supports authorization through [webhooks](../local-integrations/local-webhooks.md) and the [OAuth protocol](../settings/oauth/index.md), checks types and parameters of requests before sending, returns data in native Python data structures, and standardizes API error handling.

The SDK includes integrations for Django, FastAPI, and Flask: they help validate the data that Bitrix24 sends when opening an application, handling events, and working with business processes.

Use B24PySDK:

- if you are developing an application, integration, or automation in Python,
- if you need to work with the Bitrix24 REST API without manually assembling HTTP requests,
- if IDE autocompletion, request parameter validation, and predictable response structures are important,
- if you plan a backend application that needs to reliably handle authorization, events, and API errors.

## Go SDK

[B24GoSDK](./b24gosdk/index.md) is the official Go SDK for the Bitrix24 REST API. It handles authorization, retries on failures, list traversal, and response parsing, while a REST method is called by its name. There are deliberately no wrappers for individual methods: a method that Bitrix24 releases today is available in the SDK on the same day. Authorization is available through [webhooks](../local-integrations/local-webhooks.md) and the [OAuth protocol](../settings/oauth/index.md).

Use B24GoSDK:

- if you are developing an integration, an application, or a background service in Go,
- if you need [batch requests](../settings/how-to-call-rest-api/batch.md) and the export of large lists,
- if predictable behavior on connection drops and rate limits is important,
- if you need code without external dependencies.

## Additional Tools

[MCP](../ai-tools/mcp.md) is an auxiliary service for AI assistants in the development environment. MCP provides AI assistants with structured data and specifications for REST API methods, helps generate correct code, and speeds up application creation.

[B24 AI Starter](https://github.com/bitrix24/b24-ai-starter) is a starter template for a Bitrix24 application with instructions for AI agents. The template includes a ready-made Nuxt 3 frontend with the UI Kit connected and three backend options to choose from: PHP, Python, or Node.js — with SDKs, authorization through the [OAuth protocol](../settings/oauth/index.md), and a Docker environment. The template saves you from building the application structure from scratch: the AI agent writes the business logic following the ready-made instructions from the repository.

[UI Kit](../api-reference/widgets/ui-kit/index.md) is a set of Vue components for creating interfaces in the Bitrix24 brand style. It ensures consistency in visual language and accelerates frontend development.