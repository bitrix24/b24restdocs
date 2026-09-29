# Event After Application Update onAppUpdate

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`basic`](../../scopes/permissions.md)
>
> Who can subscribe: any user

The `ONAPPUPDATE` event is triggered after a new version of the application is installed in Bitrix24. The handler receives the current and previous versions of the application and the `application_token`.

{% note info "" %}

Events will not be sent to the application until the installation is complete. [Check the application installation](../../../settings/app-installation/installation-finish.md).

{% endnote %}

## What the Handler Receives

Data is transmitted as a POST request {.b24-info}

```json
{
    "event": "ONAPPUPDATE",
    "event_handler_id": "12",
    "data": {
        "VERSION": "3",
        "PREVIOUS_VERSION": "2",
        "LANGUAGE_ID": "de"
    },
    "ts": "1696527000",
    "auth": {
        "domain": "some-domain.bitrix24.com",
        "scope": "crm,user",
        "access_token": "lh8ze36o8ulgrljbyscr36c7ay5sinva",
        "refresh_token": "5f1ih5tsnsb11sc5heg3kp4ywqnjhd09",
        "expires_in": "3600",
        "server_endpoint": "https://oauth.bitrix.info/rest/",
        "status": "F",
        "client_endpoint": "https://some-domain.bitrix24.com/rest/",
        "member_id": "d41d8cd98f00b204e9800998ecf8427e",
        "application_token": "c917d38f6bdb84e9d9e0bfe9d585be73"
    }
}
```

## Request Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **event***
[`string`](../../data-types.md) | Symbolic event code. In this case — `ONAPPUPDATE` ||
|| **event_handler_id**
[`integer`](../../data-types.md) | Event handler ID ||
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

#|
|| **Name**
`type` | **Description** ||
|| **VERSION***
[`string`](../../data-types.md) | Current installed version of the application ||
|| **PREVIOUS_VERSION***
[`string`](../../data-types.md) | Previous version before the update ||
|| **LANGUAGE_ID***
[`string`](../../data-types.md) | Default language of the Bitrix24 account: `ru`, `en` and others ||
|#

### Parameter auth {#auth}

#|
|| **Name**
`type` | **Description** ||
|| **domain***
[`string`](../../data-types.md) | Address of the Bitrix24 account where the event occurred ||
|| **scope**
[`string`](../../data-types.md) | Codes of the [permissions](../../scopes/permissions.md) granted to the application, separated by commas ||
|| **access_token**
[`string`](../../data-types.md) | OAuth 2.0 authorization token ||
|| **refresh_token**
[`string`](../../data-types.md) | Token for extending OAuth 2.0 authorization ||
|| **expires_in**
[`integer`](../../data-types.md) | Access token lifetime in seconds ||
|| **server_endpoint***
[`string`](../../data-types.md) | Authorization server address for token renewal ||
|| **status**
[`string`](../../data-types.md) | Status of the application that subscribed to this event:

- `L` — local application
- `F` — free mass-market application
- `D` — demo version of a mass-market application
- `T` — trial version of a mass-market application, time-limited
- `P` — paid mass-market application
||
|| **client_endpoint***
[`string`](../../data-types.md) | Common path for API method calls to the account ||
|| **member_id***
[`string`](../../data-types.md) | Unique identifier of the account ||
|| **application_token***
[`string`](../../data-types.md) | Application token. Compare it with the token retained during installation to make sure the request came from Bitrix24. For more details, see the article [{#T}](../../events/safe-event-handlers.md) ||
|#

If the event could be linked to a user, `auth` contains `access_token`, `refresh_token`, `expires_in`, `scope`, and `status`, [more details](../../events/index.md#auth).

## Continue Learning

- [{#T}](../../events/index.md)
- [{#T}](../../events/event-bind.md)
- [{#T}](./index.md)
- [{#T}](./on-app-user-ready.md)
- [{#T}](./on-app-install.md)
- [{#T}](./on-app-payment.md)
- [{#T}](./on-app-method-confirm.md)
- [{#T}](./on-user-add.md)
- [{#T}](./on-app-uninstall.md)
