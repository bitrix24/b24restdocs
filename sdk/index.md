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

## Additional Tools

[MCP](./mcp.md) is an auxiliary service for AI assistants in the development environment. MCP provides AI assistants with structured data and specifications for REST API methods, helps generate correct code, and speeds up application creation.

[UI Kit](../api-reference/widgets/ui-kit/index.md) is a set of Vue components for creating interfaces in the Bitrix24 brand style. It ensures consistency in visual language and accelerates frontend development.