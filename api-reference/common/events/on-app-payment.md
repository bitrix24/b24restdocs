# Event onAppPayment

> Scope: [`basic`](../../scopes/permissions.md)
>
> Who can subscribe: any user

The `onAppPayment` event is triggered when an application is paid for.

{% note info "" %}

Events will not be sent to the application until the installation is complete. [Check the application installation](../../../settings/app-installation/installation-finish.md)

{% endnote %}

## What the handler receives

Data is transmitted as a POST request {.b24-info}

```
{
    "event": "ONAPPPAYMENT",
    "data": {
        "CODE": "bitrix.gds_company",
        "VERSION": 1,
        "STATUS": "S",
        "PAYMENT_EXPIRED": "N",
        "DAYS": 28,
        "LANGUAGE_ID": "de",
    },
    "ts": "1466439714",
    "auth": {
        "domain": "some-domain.bitrix24.com",
        "server_endpoint": "https://oauth.bitrix.info/rest/", 
        "client_endpoint": "https://some-domain.bitrix24.com/rest/", 
    }
}
```

## Request parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **event*** 
[`string`](../../data-types.md) | Symbolic event code — `ONAPPPAYMENT` ||
|| **data*** 
[`array`](../../data-types.md) | Payment data.

The structure is described [below](#data) ||
|| **ts*** 
[`timestamp`](../../data-types.md) | Date and time of the event sent from the queue ||
|| **auth*** 
[`array`](../../data-types.md) | Authorization and account data.

The structure is described [below](#auth) ||
|#

### Parameter data {#data}

{% include [Note on required parameters](../../../_includes/required.md) %}

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
- `T` (Trial) — time-limited trial version
- `P` (Paid) — paid application ||
|| **PAYMENT_EXPIRED*** 
[`string`](../../data-types.md) | [Y\|N] Flag indicating whether the paid period or trial period has expired ||
|| **DAYS*** 
[`integer`](../../data-types.md) | Number of days remaining until the end of the paid period or trial period ||
|| **LANGUAGE_ID*** 
[`string`](../../data-types.md) | Installed language: `de`, `en`, and others ||
|#

### Parameter auth {#auth}

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **domain*** 
[`string`](../../data-types.md) | Bitrix24 account address ||
|| **server_endpoint*** 
[`string`](../../data-types.md) | Authorization server address for token renewal ||
|| **client_endpoint*** 
[`string`](../../data-types.md) | Common path for API method calls to the account ||
|| **member_id*** 
[`string`](../../data-types.md) | Unique account identifier ||
|#

## Continue exploring

- [{#T}](../../events/index.md)
- [{#T}](../../events/event-bind.md)
- [{#T}](../system/app-info.md)
- [{#T}](./on-app-install.md)
- [{#T}](./on-app-method-confirm.md)
- [{#T}](./on-user-add.md)
- [{#T}](./on-app-uninstall.md)