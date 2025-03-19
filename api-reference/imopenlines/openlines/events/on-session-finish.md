# OnSessionFinish Chat Closure

> Scope: [`imopenlines`](../../../scopes/permissions.md) 
>
> Who can subscribe: any user

The `OnSessionFinish` event triggers when a chat is closed.

[Subscribe](../../../events/event-bind.md) to the event can only be done through the application. Only those events intended for the [connector](../../imconnector/index.md) added by the application can be received in the handler.

## What the handler receives

Data is transmitted as a POST request

```php
[
    'event' => 'ONSESSIONFINISH',
    'eventId' => 1,
    'data' => [
        'DATA' => [
            [
                'connector' => [
                    'connector_id' => 'livechat',
                    'line_id' => 128,
                    'chat_id' => 10585,
                    'user_id' => 1984,
                ],
                'chat' => [
                    'id' => 10585
                ],
                'user' => [
                    'id' => 128,
                    'name' => 'linename'
                ],
            ],
        ],
    ],
    'ts' => 1714649632,
    'auth' => [
        'access_token' => 's6p6eclrvim6da22ft9ch94ekreb52lv',
        'expires_in' => 3600,
        'scope' => 'imopenlines',
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
|| **DATA***  
[`object`](../../../data-types.md) | Object with [chat data](#chat-params) ||
|#

#### Parameter DATA {#chat-params}

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **connector***  
[`object`](../../../data-types.md) | Object with information about the connector:
- `connector_id` — connector identifier
- `line_id` — open line identifier
- `chat_id` — chat identifier
- `user_id` — user identifier in the external system
||
|| **chat***  
[`object`](../../../data-types.md) | Object with information about the chat:
- `id` — chat identifier ||
|| **line***  
[`object`](../../../data-types.md) | Object with information about the open line:
- `id` — open line identifier
- `name` — name of the open line ||
|#

### Parameter auth

{% include notitle [Parameter auth](../../../../_includes/auth-params-in-events.md) %}