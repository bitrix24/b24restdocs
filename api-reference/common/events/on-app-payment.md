# Event on Application Payment onAppPayment

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`basic`](../../scopes/permissions.md)
>
> Who can subscribe: any user

The `ONAPPPAYMENT` event is triggered when an application is paid for.

{% note info "" %}

Events will not be sent to the application until the installation is complete. [Check the application installation](../../../settings/app-installation/installation-finish.md)

{% endnote %}

## What the Handler Receives

Data is transmitted as a POST request {.b24-info}

```json
{
    "event": "ONAPPPAYMENT",
    "data": {
        "CODE": "bitrix.gds_company",
        "VERSION": 1,
        "STATUS": "S",
        "PAYMENT_EXPIRED": "N",
        "DAYS": 28,
        "LANGUAGE_ID": "de"
    },
    "ts": "1466439714",
    "auth": {
        "domain": "some-domain.bitrix24.com",
        "server_endpoint": "https://oauth.bitrix.info/rest/",
        "client_endpoint": "https://some-domain.bitrix24.com/rest/",
        "member_id": "a223c6b3710f85df22e9377d6c4f7553"
    }
}
```

## Request Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **event***
[`string`](../../data-types.md) | Event character code — `ONAPPPAYMENT` ||
|| **data***
[`object`](../../data-types.md) | Payment data.

The structure is described [below](#data) ||
|| **ts***
[`timestamp`](../../data-types.md) | Date and time of the event sent from the queue ||
|| **auth***
[`object`](../../data-types.md) | Authorization and account data.

The structure is described [below](#auth) ||
|#

### Parameter data {#data}

#|
|| **Name**
`type` | **Description** ||
|| **CODE***
[`string`](../../data-types.md) | Application code ||
|| **VERSION***
[`integer`](../../data-types.md) | Installed application version ||
|| **STATUS***
[`string`](../../data-types.md) | Application status. Possible values:
- `F` (Free) — free
- `D` (Demo) — demo version
- `T` (Trial) — trial version, time-limited
- `P` (Paid) — paid application ||
|| **PAYMENT_EXPIRED***
[`string`](../../data-types.md) | [Y|N] Flag indicating whether the paid period or trial period has expired ||
|| **DAYS***
[`integer`](../../data-types.md) | Number of days remaining until the end of the paid period or trial period ||
|| **LANGUAGE_ID***
[`string`](../../data-types.md) | Set language: `ru`, `en` and others ||
|#

### Parameter auth {#auth}

#|
|| **Name**
`type` | **Description** ||
|| **domain***
[`string`](../../data-types.md) | Address of the Bitrix24 account where the event occurred ||
|| **server_endpoint***
[`string`](../../data-types.md) | Authorization server address for token renewal||
|| **client_endpoint***
[`string`](../../data-types.md) | Common path for API method calls to the account ||
|| **member_id***
[`string`](../../data-types.md) | Unique identifier of the account ||
|#

## Continue Learning

- [{#T}](../../events/index.md)
- [{#T}](../../events/event-bind.md)
- [{#T}](../system/app-info.md)
- [{#T}](./on-app-install.md)
- [{#T}](./on-app-method-confirm.md)
- [{#T}](./on-user-add.md)
- [{#T}](./on-app-uninstall.md)
