# OnOpenLineMessageDelete Chat Message Deletion

> Scope: [`imopenlines`](../../../scopes/permissions.md) 
>
> Who can subscribe: any user

The `OnOpenLineMessageDelete` event is triggered when a message is deleted in the open line chat.

[Subscribe](../../../events/event-bind.md) to the event can only be done through the application. The handler can only receive those events that are intended for the [connector](../../imconnector/index.md) added by the application.

## What the handler receives

Data is transmitted in the form of a POST request

```php
[
    'event' => 'ONOPENLINEMESSAGEDELETE',
    'eventId' => 1,
    'data' => [
        'CONNECTOR' => 'livechat',
        'LINE' => 128,
        'DATA' => [
            [
                'im' => [
                    'chat_id' => 1024,
                    'message_id' => 2056,
                ],
                'message' => [
                    'id' => 2056,
                ],
                'chat' => [
                    'id' => 1024
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
|| **CONNECTOR*** 
[`string`](../../../data-types.md) | Connector identifier ||
|| **LINE*** 
[`integer`](../../../data-types.md) | Open line identifier ||
|| **DATA*** 
[`object`](../../../data-types.md) | Object with [chat data](#chat-params) ||
|#

#### Parameter DATA {#chat-params}

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **im*** 
[`object`](../../../data-types.md) | Object with information about the deleted message in the chat:
- `chat_id` — chat identifier
- `message_id` — message identifier
||
|| **message*** 
[`object`](../../../data-types.md) | Object with information about the message:
- `id` — message identifier
||
|| **chat*** 
[`object`](../../../data-types.md) | Object with information about the chat:
- `id` — chat identifier ||
|#

### Parameter auth

{% include notitle [Parameter auth](../../../../_includes/auth-params-in-events.md) %}