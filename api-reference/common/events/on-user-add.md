# Event onUserAdd

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`user`](../../scopes/permissions.md), [`user_brief`](../../scopes/permissions.md), or [`user_basic`](../../scopes/permissions.md)
>
> Who can subscribe: any user

The `ONUSERADD` event is triggered when a user is added to Bitrix24. The event occurs not after the invitation, but after the user logs into Bitrix24 and completes the registration. The event is not sent for external user types, such as email users and bots.

You can subscribe to the event through an [outbound webhook](../../../local-integrations/local-webhooks.md) or from an application with the [event.bind](../../events/event-bind.md) method. OAuth 2.0 tokens are not passed to an outbound webhook.

{% note info "" %}

Events will not be sent to the application until the installation is complete. [Check the application installation](../../../settings/app-installation/installation-finish.md).

{% endnote %}

## What the Handler Receives

Data is transmitted as a POST request {.b24-info}

```json
{
    "event": "ONUSERADD",
    "event_handler_id": "16",
    "data": {
        "ID": "123",
        "ACTIVE": "1",
        "EMAIL": "user@example.com",
        "NAME": "Klaus",
        "LAST_NAME": "Weber",
        "PERSONAL_GENDER": "M",
        "PERSONAL_BIRTHDAY": "1990-01-01T00:00:00+01:00",
        "UF_DEPARTMENT": ["1", "2"],
        "DATE_REGISTER": "2024-04-05T00:00:00+02:00",
        "WORK_POSITION": "Developer",
        "UF_EMPLOYMENT_DATE": "2024-04-05T00:00:00+02:00"
    },
    "ts": "1466439714",
    "auth": {
        "access_token": "s6p6eclrvim6da22ft9ch94ekreb52lv",
        "expires_in": "3600",
        "scope": "user",
        "domain": "some-domain.bitrix24.com",
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
[`string`](../../data-types.md) | Event character code — `ONUSERADD` ||
|| **event_handler_id**
[`integer`](../../data-types.md) | Event handler ID ||
|| **data***
[`object`](../../data-types.md) | Data of the added user.

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
|| **ID***
[`integer`](../../data-types.md) | User identifier ||
|| **ACTIVE***
[`string`](../../data-types.md) | Whether the user is active.

Possible values:

- `1` — active
- `0` — inactive

The event is triggered on the user's first login, so in practice `1` is passed
||
|| **EMAIL**
[`string`](../../data-types.md) | User's email ||
|| **NAME**
[`string`](../../data-types.md) | User's first name ||
|| **LAST_NAME**
[`string`](../../data-types.md) | User's last name ||
|| **PERSONAL_GENDER**
[`string`](../../data-types.md) | Gender: `M` — male, `F` — female ||
|| **PERSONAL_BIRTHDAY**
[`string`](../../data-types.md) | Date of birth in ISO 8601 format, for example `1990-01-01T00:00:00+01:00` ||
|| **UF_DEPARTMENT**
[`array`](../../data-types.md) | Array of department `ID`s. May be absent for extranet users ||
|| **DATE_REGISTER***
[`string`](../../data-types.md) | Registration date in ISO 8601 format ||
|| **WORK_POSITION**
[`string`](../../data-types.md) | Position of the user ||
|| **UF_EMPLOYMENT_DATE**
[`string`](../../data-types.md) | Employment date in ISO 8601 format ||
|#

{% note info "" %}

The table lists the main fields. The set of fields depends on the application permissions:

- with the `user` scope, standard user fields are passed, custom fields `UF_USR_*` are not passed
- with `user_basic` or `user_brief`, a reduced set of fields is passed, and together with `user.userfield`, custom fields `UF_USR_*` as well

Fields the application has no access to and fields without a value are not passed. For field descriptions, see the article [{#T}](../../user/user-fields.md).

{% endnote %}

### Parameter auth {#auth}

#|
|| **Name**
`type` | **Description** ||
|| **access_token**
[`string`](../../data-types.md) | Token for accessing the API ||
|| **expires_in**
[`integer`](../../data-types.md) | Time in seconds until the token expires ||
|| **scope**
[`string`](../../data-types.md) | Codes of the [permissions](../../scopes/permissions.md) granted to the application, separated by commas ||
|| **domain***
[`string`](../../data-types.md) | Address of Bitrix24 where the event occurred ||
|| **server_endpoint***
[`string`](../../data-types.md) | Authorization server address for token renewal ||
|| **status**
[`string`](../../data-types.md) | Status of the application subscribed to this event:

- `L` — [local](../../../local-integrations/local-apps.md) application
- `F` — [free mass-market](../../../market/index.md) application
- `D` — demo version of a mass-market application
- `T` — trial version of a mass-market application, time-limited
- `P` — paid mass-market application

||
|| **client_endpoint***
[`string`](../../data-types.md) | Common path for API method calls for Bitrix24 where the event occurred ||
|| **member_id***
[`string`](../../data-types.md) | Unique identifier of the account ||
|| **application_token***
[`string`](../../data-types.md) | Application token. Compare it with the token retained during the application installation or with the token from the outbound webhook settings to make sure the request came from Bitrix24. For more details, see the article [{#T}](../../events/safe-event-handlers.md) ||
|#

If the event could be linked to a user, `auth` contains `access_token`, `expires_in`, `scope`, and `status`, [more details](../../events/index.md#auth).

## Continue Learning

- [{#T}](../../events/index.md)
- [{#T}](../../events/event-bind.md)
- [{#T}](./index.md)
- [{#T}](../../user/user-get.md)
- [{#T}](../../user/user-fields.md)
- [{#T}](./on-app-install.md)
- [{#T}](./on-app-payment.md)
- [{#T}](./on-app-method-confirm.md)
- [{#T}](./on-app-uninstall.md)
