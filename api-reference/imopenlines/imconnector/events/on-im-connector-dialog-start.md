# When Creating a Dialog OnImConnectorDialogStart

> Scope: [`imconnector`](../../../scopes/permissions.md) 
>
> Who can subscribe: any user

The event is triggered when a dialog is created.

## What the Handler Receives

Data is sent as a POST request

```php
[
    'event' => 'ONIMCONNECTORDIALOGSTART',
    'eventId' => 1,
    'data' => [
        'CONNECTOR' => 'newcustomconnector',
        'LINE' => '105',
        'DATA' => [
            [
                'connector' => [
                    'connector_id' => 'newcustomconnector',
                    'line_id' => 105,
                    'chat_id' => 8,
                    'user_id' => 0,
                ],
                'session' => [
                    'id' => 3282,
                    'closed' => N,
                    'parent_id' => 0,
                    'close_term' => 10,
                ],
                'chat' => [
                    'id' => 8
                ],
                'user' => [
                    'id' => 0
                ],
            ],
        ],
    ],
    'ts' => 1714649632,
    'auth' => [
        'access_token' => 's6p6eclrvim6da22ft9ch94ekreb52lv',
        'expires_in' => 3600,
        'scope' => 'imconnector',
        'domain' => 'some-domain.bitrix24.com',
        'server_endpoint' => 'https://oauth.bitrix.info/rest/&#39;',
        'status' => 'F',
        'client_endpoint' => 'https://some-domain.bitrix24.com/rest/&#39;',
        'member_id' => 'a223c6b3710f85df22e9377d6c4f7553',
        'refresh_token' => '4s386p3q0tr8dy89xvmt96234v3dljg8',
        'application_token' => '51856fefc120afa4b628cc82d3935cce',
    ],
]
```

## Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **event***  
[`string`](../../../data-types.md) | Symbolic event code ||
|| **eventId***  
[`integer`](../../../data-types.md) | Event identifier ||
|| **data***  
[`object`](../../../data-types.md) | Object with [event data](#data) ||
|| **ts***  
[`integer`](../../../data-types.md) | Timestamp of the event sent from the event queue ||
|| **auth***  
[`object`](../../../data-types.md) | Object with authorization parameters and information about the account where the event occurred ||
|#

### Parameter data {#data}

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CONNECTOR***  
[`string`](../../../data-types.md) | Connector identifier ||
|| **LINE***  
[`integer`](../../../data-types.md) | Open line identifier ||
|| **DATA***  
[`object`](../../../data-types.md) | Object with [dialog data](#dialog-params) ||
|#

#### Parameter DATA {#dialog-params}

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **connector***  
[`object`](../../../data-types.md) | Object with information about additional settings:
- `connector_id` — connector identifier
- `line_id` — open line identifier
- `chat_id` — chat identifier
- `user_id` — user identifier in the external system
||
|| **session***  
[`object`](../../../data-types.md) | Object with information about the session:
- `id` — session identifier
- `closed` — mark indicating if the dialog is closed. `N` — dialog is open
- `parent_id` — identifier of the previous session
- `close_term` — number of minutes until the session closes ||
|| **chat***  
[`object`](../../../data-types.md) | Object with information about the chat:
- `id` — chat identifier ||
|| **user***  
[`object`](../../../data-types.md) | Object with information about the user:
- `id` — user identifier in the external system ||
|#

### Parameter auth

{% include notitle [Parameter auth](../../../../_includes/auth-params-in-events.md) %}