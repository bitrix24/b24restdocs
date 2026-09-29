# Event After Successful Application Installation onAppInstall

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`basic`](../../scopes/permissions.md)
>
> Who can subscribe: any user

The `ONAPPINSTALL` event is triggered immediately after the successful installation of an application on Bitrix24. An application without an interface also receives the event when it is updated.

{% note info "" %}

Events will not be sent to the application until the installation is complete. [Check the application installation](../../../settings/app-installation/installation-finish.md).

{% endnote %}

## What the Handler Receives

Data is transmitted as a POST request {.b24-info}

```json
{
    "event": "ONAPPINSTALL",
    "event_handler_id": "11",
    "data": {
        "VERSION": "1",
        "ACTIVE": "Y",
        "INSTALLED": "Y",
        "LANGUAGE_ID": "de"
    },
    "ts": "1696527000",
    "auth": {
        "domain": "some-domain.bitrix24.com",
        "scope": "crm,user",
        "access_token": "s6p6eclrvim6da22ft9ch94ekreb52lv",
        "refresh_token": "4s386p3q0tr8dy89xvmt96234v3dljg8",
        "expires_in": "3600",
        "server_endpoint": "https://oauth.bitrix.info/rest/",
        "status": "F",
        "client_endpoint": "https://some-domain.bitrix24.com/rest/",
        "member_id": "a223c6b3710f85df22e9377d6c4f7553",
        "application_token": "51856fefc120afa4b628cc82d3935cce"
    }
}
```

## Request Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **event***
[`string`](../../data-types.md) | Symbolic event code. In this case — `ONAPPINSTALL` ||
|| **event_handler_id**
[`integer`](../../data-types.md) | Event handler ID ||
|| **data***
[`object`](../../data-types.md) | Data about the installed application.

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
[`string`](../../data-types.md) | Number of the installed application version, for example `1` ||
|| **ACTIVE***
[`string`](../../data-types.md) | Application activity status. Always `Y`: events are not sent to inactive applications ||
|| **INSTALLED***
[`string`](../../data-types.md) | Whether the application is ready for use. Always `Y`: events are not sent until the installation is complete ||
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
- `P` — paid mass-market application ||
|| **client_endpoint***
[`string`](../../data-types.md) | Common path for API method calls to the account ||
|| **member_id***
[`string`](../../data-types.md) | Unique identifier of the account ||
|| **application_token***
[`string`](../../data-types.md) | Application token. Retain it: handlers of other events use it to verify that the request came from Bitrix24. For more details, see the article [{#T}](../../events/safe-event-handlers.md) ||
|#

If the event could be linked to a user, `auth` contains `access_token`, `refresh_token`, `expires_in`, `scope`, and `status`, [more details](../../events/index.md#auth).

{% note info "" %}

If the application has no interface and an installation URL is specified, Bitrix24 automatically registers the `ONAPPINSTALL` handler on this URL during installation.

You can set a handler on a different URL with the [event.bind](../../events/event-bind.md) method in the installation script of the application. The script URL is specified in a separate field of the application version card.

{% endnote %}

## Continue Learning

- [{#T}](../../events/index.md)
- [{#T}](../../events/event-bind.md)
- [{#T}](./index.md)
- [{#T}](./on-app-user-ready.md)
- [{#T}](./on-app-update.md)
- [{#T}](./on-app-uninstall.md)
- [{#T}](./on-app-payment.md)
- [{#T}](../../../settings/app-installation/installation-finish.md)
