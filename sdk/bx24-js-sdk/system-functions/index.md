# Initialization and Authorization: Feature Overview

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

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

## Key Considerations

- The functions work only inside the application frame. The library receives Bitrix24 data from the parent page, so calls made outside the application do not go through
- [BX24.getAuth](./bx24-get-auth.md) returns the result immediately: either an object with authorization data or `false` if the library is not initialized yet or the token has expired
- The other functions of this section do not return data. The result arrives in a callback function
- The functions require no scope of their own: they do not call the REST API. Permissions and scope are checked in the Bitrix24 methods you call after initialization
- The token is refreshed automatically on the next call to a Bitrix24 method. Call [BX24.refreshAuth](./bx24-refresh-auth.md) only if your code needs the token before such a call

## Authorization Data

[BX24.getAuth](./bx24-get-auth.md) and [BX24.refreshAuth](./bx24-refresh-auth.md) return the same set of fields:

#|
|| **Field** | **Description** ||
|| **access_token** | Access token. The library substitutes it into requests to Bitrix24 on its own ||
|| **refresh_token** | Refresh token. It is required to obtain a new `access_token` after the current one expires ||
|| **expires_in** | The moment the access token expires ||
|| **domain** | The address of the Bitrix24 where the application is open, for example `mycompany.bitrix24.com` ||
|| **member_id** | Permanent identifier of the Bitrix24 account. It links an application installation to a record on your side ||
|#

## Relationship with Other Objects

**Application.** Authorization data is issued to an installed application. The `member_id` and `domain` identifiers from [BX24.getAuth](./bx24-get-auth.md) show which Bitrix24 the application runs in — they are retained on your side to link a user to an installation. The installation procedure is described in the [Application Installation](../../../settings/app-installation/index.md) section.

**Bitrix24 Methods.** After initialization, the library substitutes the token into requests on its own, so passing `auth` separately is not required. The [Calling REST Methods](../how-to-call-rest-methods/index.md) section helps you call methods from the client side of the application.

**App Configurations.** The values that an application retains in Bitrix24 between launches are read and written by the functions of the [App Configurations](../options/index.md) section. It is convenient to fill them in within the [BX24.install](./bx24-install.md) first launch handler.

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