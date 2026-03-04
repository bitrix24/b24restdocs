# Event on Receiving Permission for Using onAppMethodConfirm Methods

> Scope: [`basic`](../../scopes/permissions.md)
>
> Who can subscribe: any user

The `onAppMethodConfirm` event is triggered upon receiving the [administrator's decision](../../scopes/confirmation.md) from the account regarding the use of methods.

{% note info "" %}

Events will not be sent to the application until the installation is complete. [Check the application installation](../../../settings/app-installation/installation-finish.md)

{% endnote %}

## What the Handler Receives

Data is transmitted as a POST request {.b24-info}

```json
{
    "event": "ONAPPMETHODCONFIRM",
    "data": {
        "TOKEN": "fkp963yuv1ggkfbs5z3f5hy8lilm0iw6",
        "METHOD": "voximplant.user.get",
        "CONFIRMED": "1",
        "LANGUAGE_ID": "en"
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

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **event***
[`string`](../../data-types.md) | Event character code — `ONAPPMETHODCONFIRM` ||
|| **data***
[`object`](../../data-types.md) | Data about the permitted methods.

The structure is described [below](#data) ||
|| **ts***
[`timestamp`](../../data-types.md) | Date and time of event sending ||
|| **auth***
[`object`](../../data-types.md) | Authorization and account data.

The structure is described [below](#auth) ||
|#

### Parameter data {#data}

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **TOKEN***
[`string`](../../data-types.md) | Authorization token with which permission was requested ||
|| **METHOD***
[`string`](../../data-types.md) | API method for which permission was requested ||
|| **CONFIRMED***
[`string`](../../data-types.md) | Permission result: `0` — denied, `1` — granted ||
|| **LANGUAGE_ID***
[`string`](../../data-types.md) | Set language: `en`, `de`, and others ||
|#

### Parameter auth {#auth}

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **domain***
[`string`](../../data-types.md) | Address of the Bitrix24 account ||
|| **server_endpoint***
[`string`](../../data-types.md) | Authorization server address for token renewal ||
|| **client_endpoint***
[`string`](../../data-types.md) | Common path for API method calls to the account ||
|| **member_id***
[`string`](../../data-types.md) | Unique identifier of the account ||
|| **application_token***
[`string`](../../data-types.md) | Token for secure event processing ||
|#

## Continue Learning

- [{#T}](../../events/index.md)
- [{#T}](../../events/event-bind.md)
- [{#T}](./on-app-install.md)
- [{#T}](./on-app-payment.md)
- [{#T}](./on-user-add.md)
- [{#T}](./on-app-uninstall.md)