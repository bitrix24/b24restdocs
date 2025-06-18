# Event after successful application installation OnAppInstall

> Scope: [`basic`](../../scopes/permissions.md)
>
> Who can subscribe: any user

The `OnAppInstall` event is triggered immediately after the successful installation of an application on Bitrix24. The `application_token` is passed to the handler, which is important to save. For more details, refer to the article [{#T}](../../events/safe-event-handlers.md).

## What the handler receives

Data is sent as a POST request {.b24-info}

```
array(
    'event' => 'ONAPPINSTALL',
    'data' => array(
        'VERSION' => '1.0.0',
        'ACTIVE' => 'Y',
        'INSTALLED' => 'Y',
        'LANGUAGE_ID' => 'de'
    ),
    'ts' => '1696527000',
    'auth' => array(
        'domain' => 'some-domain.bitrix24.com',
        'server_endpoint' => 'https://oauth.bitrix.info/rest/',   
        'status' => 'F',
        'client_endpoint' => 'https://some-domain.bitrix24.com/rest/',   
        'member_id' => 'a223c6b3710f85df22e9377d6c4f7553',
        'application_token' => '51856fefc120afa4b628cc82d3935cce',        
    ),
)
```

## Request parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **event***
[`string`](../../data-types.md) | Symbolic code of the event. In this case — `ONAPPINSTALL` ||
|| **data***
[`object`](../../data-types.md) | Data about the installed application.

The structure is described [below](#data) ||
|| **ts***
[`timestamp`](../../data-types.md) | Date and time of the event sent from the queue ||
|| **auth***
[`object`](../../data-types.md) | Object containing authorization parameters and data about the account where the event occurred.

The structure is described [below](#auth) ||
|#

### Parameter data {#data}

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **LANGUAGE_ID***
[`string`](../../data-types.md) | Installed language: `de`, `en`, and others ||
|| **VERSION***
[`integer`](../../data-types.md) | Version of the installed application ||
|| **ACTIVE***
[`string`](../../data-types.md) | Status of the application's activity. 

Possible values:
`Y` — active
`N` — inactive ||
|| **INSTALLED***
[`string`](../../data-types.md) | Is the application ready for use. 

Possible values: 
`Y` — ready
`N` — not fully installed ||
|#

### Parameter auth {#auth}

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **domain***
[`string`](../../data-types.md) | Address of the Bitrix24 account ||
|| **server_endpoint***
[`string`](../../data-types.md) | Address of the authorization server for token renewal ||
|| **client_endpoint***
[`string`](../../data-types.md) | General path for API method calls of the account ||
|| **member_id***
[`string`](../../data-types.md) | Unique identifier of the account ||
|| **application_token***
[`string`](../../data-types.md) | Token for secure event handling ||
|#

{% note warning "" %}

 The handler for this event can be set in the installation script of the application, which is specified in the version card in a separate field.

{% endnote %}

## Continue your study

- [{#T}](../../events/index.md)
- [{#T}](../../events/event-bind.md)
- [{#T}](./on-app-payment.md)
- [{#T}](./on-app-method-confirm.md)
- [{#T}](./on-user-add.md)
- [{#T}](./on-app-uninstall.md)