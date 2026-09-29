# Event on Application Payment onAppPayment

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`basic`](../../scopes/permissions.md)
>
> Who can subscribe: any user

The `ONAPPPAYMENT` event is triggered when an application is paid for or its paid period is extended. Bitrix24 receives the new status from the Market, for example, when a user opens the application or the list of installed applications. Therefore, the event may arrive later than the payment itself. The event is sent only to the application that was paid for.

{% note info "" %}

Events will not be sent to the application until the installation is complete. [Check the application installation](../../../settings/app-installation/installation-finish.md).

{% endnote %}

## What the Handler Receives

Data is transmitted as a POST request {.b24-info}

```json
{
    "event": "ONAPPPAYMENT",
    "event_handler_id": "14",
    "data": {
        "CODE": "bitrix.gds_company",
        "VERSION": "1",
        "STATUS": "P",
        "PAYMENT_EXPIRED": "N",
        "DAYS": "28"
    },
    "ts": "1466439714",
    "auth": {
        "domain": "some-domain.bitrix24.com",
        "server_endpoint": "https://oauth.bitrix.info/rest/",
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
[`string`](../../data-types.md) | Event character code — `ONAPPPAYMENT` ||
|| **event_handler_id**
[`integer`](../../data-types.md) | Event handler ID ||
|| **data***
[`object`](../../data-types.md) | Data about the application payment status.

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
|| **CODE***
[`string`](../../data-types.md) | Application code ||
|| **VERSION***
[`string`](../../data-types.md) | Installed application version ||
|| **STATUS***
[`string`](../../data-types.md) | Application status in Bitrix24.

Possible values:

- `L` — local application
- `F` — free mass-market application
- `D` — demo version of a mass-market application
- `T` — trial version of a mass-market application, time-limited
- `P` — paid mass-market application
||
|| **PAYMENT_EXPIRED***
[`string`](../../data-types.md) | Whether the paid period of the application has expired. Always `N`: the event is not sent when the period has expired ||
|| **DAYS**
[`integer`](../../data-types.md) | Number of days remaining until the end of the paid period. If the application has no period end date, the key is not passed ||
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

If the event could be linked to a user, `auth` additionally contains `access_token`, `expires_in`, `scope`, and `status`, [more details](../../events/index.md#auth). The `refresh_token` key is not passed for this event.

## Continue Learning

- [{#T}](../../events/index.md)
- [{#T}](../../events/event-bind.md)
- [{#T}](./index.md)
- [{#T}](../system/app-info.md)
- [{#T}](./on-app-install.md)
- [{#T}](./on-app-method-confirm.md)
- [{#T}](./on-user-add.md)
- [{#T}](./on-app-uninstall.md)
