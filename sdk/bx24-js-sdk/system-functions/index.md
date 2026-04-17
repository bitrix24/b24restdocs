# Initialization and Authorization: Feature Overview

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

System functions prepare the embedded application for operation. They are used prior to API calls and interface interactions.

{% note info "" %}

The functions in this section work only within the context of the [application](../../../settings/app-installation/index.md).

{% endnote %}

> Quick Navigation: [All Functions](#all-methods)

## Getting Started

1. Wait for the library to be ready via [BX24.init](./bx24-init.md) to retrieve Bitrix24 data and proceed to the next calls.
2. Register a handler through [BX24.install](./bx24-install.md) if a first launch scenario for the current user is needed.
3. Complete the setup by calling [BX24.installFinish](./bx24-install-finish.md) to transition to the standard application initialization.
4. Obtain the current OAuth data via [BX24.getAuth](./bx24-get-auth.md) or refresh it through [BX24.refreshAuth](./bx24-refresh-auth.md).

## Feature Overview {#all-methods}

### Initialization and First Launch

#| 
|| **Function** | **Description** ||
|| [BX24.init](./bx24-init.md) | Adds an event handler for "library ready for use" ||
|| [BX24.install](./bx24-install.md) | Registers a handler for the first launch of the application for the current user ||
|| [BX24.installFinish](./bx24-install-finish.md) | Completes the work of the installer or application setup wizard ||
|#

### Application Authorization

#| 
|| **Function** | **Description** ||
|| [BX24.getAuth](./bx24-get-auth.md) | Retrieves the current authorization data via OAuth 2.0 ||
|| [BX24.refreshAuth](./bx24-refresh-auth.md) | Forcefully refreshes the authorization key ||
|#