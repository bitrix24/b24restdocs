# Event on the Administrator's Decision on a Request for a Method Requiring Confirmation onAppMethodConfirm

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`basic`](../../scopes/permissions.md)
>
> Who can subscribe: any user

The `ONAPPMETHODCONFIRM` event is triggered when the Bitrix24 administrator allows or denies the application a call to a [method requiring confirmation](../../scopes/confirmation.md). The event is sent only to the application that requested the permission.

{% note info "" %}

Events will not be sent to the application until the installation is complete. [Check the application installation](../../../settings/app-installation/installation-finish.md).

{% endnote %}

## What the Handler Receives

Data is transmitted as a POST request {.b24-info}

```json
{
    "event": "ONAPPMETHODCONFIRM",
    "event_handler_id": "15",
    "data": {
        "TOKEN": "fkp963yuv1ggkfbs5z3f5hy8lilm0iw6",
        "METHOD": "voximplant.user.get",
        "CONFIRMED": "1",
        "LANGUAGE_ID": "de"
    },
    "ts": "1478790852",
    "auth": {
        "domain": "portal.bitrix24.com",
        "client_endpoint": "https://portal.bitrix24.com/rest/",
        "server_endpoint": "https://oauth.bitrix.info/rest/",
        "member_id": "74ef8a46a75104de55d5d4a61b98ab6d",
        "application_token": "c289487163b58658eae5e8b42eaf11b8"
    }
}
```

## Request Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **event***
[`string`](../../data-types.md) | Event character code — `ONAPPMETHODCONFIRM` ||
|| **event_handler_id**
[`integer`](../../data-types.md) | Event handler ID ||
|| **data***
[`object`](../../data-types.md) | Data about the administrator's decision on the method.

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
|| **TOKEN***
[`string`](../../data-types.md) | The `access_token` value with which the application called the method and requested permission. The administrator's decision is retained for this token ||
|| **METHOD***
[`string`](../../data-types.md) | API method for which permission was requested ||
|| **CONFIRMED***
[`string`](../../data-types.md) | Permission result: `0` — denied, `1` — granted ||
|| **LANGUAGE_ID***
[`string`](../../data-types.md) | Default language of the Bitrix24 account: `ru`, `en` and others ||
|#

### Parameter auth {#auth}

#|
|| **Name**
`type` | **Description** ||
|| **domain***
[`string`](../../data-types.md) | Address of the Bitrix24 account where the event occurred ||
|| **server_endpoint***
[`string`](../../data-types.md) | Authorization server address for token renewal ||
|| **client_endpoint***
[`string`](../../data-types.md) | Common path for API method calls to the account ||
|| **member_id***
[`string`](../../data-types.md) | Unique identifier of the account ||
|| **application_token***
[`string`](../../data-types.md) | Application token. Compare it with the token retained during installation to make sure the request came from Bitrix24. For more details, see the article [{#T}](../../events/safe-event-handlers.md) ||
|#

Unlike [onAppInstall](./on-app-install.md), this event is sent without user authorization: `auth` does not contain the `access_token`, `refresh_token`, `expires_in`, `scope`, and `status` keys.

If `CONFIRMED` equals `1`, repeat the method call with the token from `data.TOKEN` before it expires.

## Continue Learning

- [{#T}](../../events/index.md)
- [{#T}](../../events/event-bind.md)
- [{#T}](./index.md)
- [{#T}](../../scopes/confirmation.md)
- [{#T}](../../events/safe-event-handlers.md)
- [{#T}](./on-app-install.md)
- [{#T}](./on-app-payment.md)
