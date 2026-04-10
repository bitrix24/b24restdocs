# Event After Application Update OnAppUpdate

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`basic`](../../scopes/permissions.md)
>
> Who can subscribe: any user

The `OnAppUpdate` event is triggered after a new version of the application is installed in Bitrix24. This event transmits information about the current and previous versions of the application, as well as the updated `application_token`. For more details, refer to the article [{#T}](../../events/safe-event-handlers.md).

## What the Handler Receives

Data is transmitted in the form of a POST request {.b24-info}

```json
{
    "event": "ONAPPUPDATE",
    "data": {
        "VERSION": "2.1.0",
        "PREVIOUS_VERSION": "2.0.3",
        "LANGUAGE_ID": "de"
    },
    "ts": "1696527000",
    "auth": {
        "domain": "some-domain.bitrix24.com",
        "scope": "imbot",
        "access_token": "lh8ze36o8ulgrljbyscr36c7ay5sinva",
        "refresh_token": "5f1ih5tsnsb11sc5heg3kp4ywqnjhd09",
        "expires_in": 3600,
        "member_id": "d41d8cd98f00b204e9800998ecf8427e",
        "application_token": "c917d38f6bdb84e9d9e0bfe9d585be73"
    }
}
```

## Request Parameters

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **event***
[`string`](../../data-types.md) | Symbolic code of the event. In this case — `ONAPPUPDATE` ||
|| **data***
[`object`](../../data-types.md) | Data about the application update.

The structure is described [below](#data) ||
|| **ts***
[`timestamp`](../../data-types.md) | Date and time of the event sent from the queue ||
|| **auth***
[`object`](../../data-types.md) | Object containing authorization parameters and information about the account where the event occurred.

The structure is described [below](#auth) ||
|#

### Parameter data {#data}

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **LANGUAGE_ID***
[`string`](../../data-types.md) | Installed language: `de`, `en`, and others ||
|| **VERSION***
[`string`](../../data-types.md) | Current installed version of the application ||
|| **PREVIOUS_VERSION***
[`string`](../../data-types.md) | Previous version before the update ||
|#

### Parameter auth {#auth}

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **domain***
[`string`](../../data-types.md) | Address of the Bitrix24 account ||
|| **scope***
[`string`](../../data-types.md) | List of permissions granted to the application, separated by spaces ||
|| **access_token***
[`string`](../../data-types.md) | OAuth 2.0 authorization token ||
|| **refresh_token***
[`string`](../../data-types.md) | Token for extending OAuth 2.0 authorization ||
|| **server_endpoint***
[`string`](../../data-types.md) | Address of the Bitrix24 authorization server, necessary for updating OAuth 2.0 tokens ||
|| **status***
[`string`](/api-reference/data-types.html) | Status of the application that subscribed to this event:

- `L` — local application
- `F` — free mass-market application
- `S` — subscription-based mass-market application
||
|| **client_endpoint***
[`string`](../../data-types.md) | Common path for API method calls to the account ||
|| **member_id***
[`string`](../../data-types.md) | Unique identifier of the account ||
|| **application_token***
[`string`](../../data-types.md) | Token for secure event handling ||
|#

## Continue Your Exploration

- [{#T}](../../events/index.md)
- [{#T}](../../events/event-bind.md)
- [{#T}](./on-app-install.md)
- [{#T}](./on-app-payment.md)
- [{#T}](./on-app-method-confirm.md)
- [{#T}](./on-user-add.md)
- [{#T}](./on-app-uninstall.md)