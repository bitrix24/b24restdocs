# Event on Application Uninstallation onAppUninstall

> Scope: [`basic`](../../scopes/permissions.md)
>
> Who can subscribe: any user

The `onAppUninstall` event is triggered when an application is uninstalled.

{% note info "" %}

Events will not be sent to the application until the installation is complete. [Check the application installation](../../../settings/app-installation/installation-finish.md)

{% endnote %}

## What the handler receives

Data is transmitted as a POST request {.b24-info}

```
{
    "event": "ONAPPUNINSTALL",
    "data": {
        "LANGUAGE_ID" => "en",
        "CLEAN": 1
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
[`string`](../../data-types.md) | Symbolic event code — `ONAPPUNINSTALL` ||
|| **data***
[`array`](../../data-types.md) | Data about the uninstalled application.

The structure is described [below](#data) ||
|| **ts***
[`timestamp`](../../data-types.md) | Date and time the event was sent from the queue ||
|| **auth***
[`array`](../../data-types.md) | Authorization and account data.

The structure is described [below](#auth) ||
|#

### Parameter data {#data}

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **LANGUAGE_ID***
[`string`](../../data-types.md) | Set language: `en`, `de`, and others ||
|| **CLEAN***
[`integer`](../../data-types.md) | Value of the "Clear application data" option set by the user when uninstalling the application. Values: `1` or `0` ||
|#

### Parameter auth {#auth}

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **domain***
[`string`](../../data-types.md) | Bitrix24 account address ||
|| **server_endpoint***
[`string`](../../data-types.md) | Authorization server address for token refresh ||
|| **client_endpoint***
[`string`](../../data-types.md) | General path for API method calls for the account ||
|| **member_id***
[`string`](../../data-types.md) | Unique identifier of the account ||
|| **application_token***
[`string`](../../data-types.md) | Token for secure event processing ||
|#

{% note warning "" %}

When an application is uninstalled, all access permissions for the application to the API are removed. Therefore, even though the event handler will receive authorization data, it can no longer use the API on behalf of the uninstalled application.

{% endnote %}

## Continue Learning

- [{#T}](../../events/index.md)
- [{#T}](../../events/event-bind.md)
- [{#T}](./on-app-install.md)
- [{#T}](./on-app-payment.md)
- [{#T}](./on-app-method-confirm.md)
- [{#T}](./on-user-add.md)