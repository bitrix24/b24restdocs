# Update Status to "Read" imconnector.send.status.reading

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- The method is hidden because it is essentially useless; the onReceivedStatusReading handler is a stub that simply returns true;

{% endnote %}

{% endif %}

> Scope: [`imopenlines`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `imconnector.send.status.reading` confirms in Bitrix24 that an outgoing message from the open line has been read in an external system. The method does not resend the message but only records the read status.

This method is not applicable for incoming messages from an external system to the open line.

{% note info "" %}

The method works only in the context of the [application](../../../settings/app-installation/index.md).

{% endnote %} 

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CONNECTOR***
[`string`](../../data-types.md) | String code of the connector specified in the `ID` parameter when calling [imconnector.register](./imconnector-register.md) ||
|| **LINE***
[`integer`](../../data-types.md) | Identifier of the open line.

The identifier can be obtained using the methods [imopenlines.config.get](../openlines/imopenlines-config-get.md) and [imopenlines.config.list.get](../openlines/imopenlines-config-list-get.md) ||
|| **MESSAGES***
[`array`](../../data-types.md) | Array of read statuses. Each element of the array is an object with blocks `im`, `message`, `chat`

The structure of the object is described in detail [below](#messages) ||
|#

### MESSAGES Parameter {#messages}

#|
|| **Name**
`type` | **Description** ||
|| **im**
[`object`](../../data-types.md) | Internal message identifiers in Bitrix24 [(detailed description)](#messages-im) ||
|| **message**
[`object`](../../data-types.md) | Message data in the external system [(detailed description)](#messages-message) ||
|| **chat**
[`object`](../../data-types.md) | Chat data in the external system [(detailed description)](#messages-chat) ||
|#

#### im Object {#messages-im}

#|
|| **Name**
`type` | **Description** ||
|| **chat_id**
[`integer`](../../data-types.md) | Identifier of the open line chat in Bitrix24 for the outgoing message ||
|| **message_id**
[`integer`](../../data-types.md) | Identifier of the message in Bitrix24 for which the "read" status needs to be set ||
|#

The fields `im.chat_id` and `im.message_id` should be obtained from the event [OnImConnectorMessageAdd](./events/on-im-connector-message-add.md) when Bitrix24 sends a message to the external system.

Typically, these values are saved when processing the event and then used to send the read status.

External identifiers `message.id` and `chat.id` do not replace `im.message_id` and `im.chat_id`.

#### message Object {#messages-message}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`array`](../../data-types.md) | Array of external message identifiers for which the read status is being sent. Even for a single message, an array should be sent, for example, `["ext-msg-1007"]` ||
|#

#### chat Object {#messages-chat}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../data-types.md) | Identifier of the chat or channel in the external system. It is recommended to send the same value as in `chat.id` of the method [imconnector.send.messages](./imconnector-send-messages.md) ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{
        "CONNECTOR": "myconnector",
        "LINE": 107,
        "MESSAGES": [
          {
            "im": {
              "chat_id": 323,
              "message_id": 85911
            },
            "message": {
              "id": ["ext-msg-1007"]
            },
            "chat": {
              "id": "channel-123"
            }
          }
        ],
        "auth": "**put_access_token_here**"
      }' \
      https://**put_your_bitrix24_address**/rest/imconnector.send.status.reading
    ```

- JS

    ```js
    const payload = {
      CONNECTOR: 'myconnector',
      LINE: 107,
      MESSAGES: [
        {
          im: { chat_id: 323, message_id: 85911 },
          message: { id: ['ext-msg-1007'] },
          chat: { id: 'channel-123' },
        },
      ],
    };

    const response = await $b24.callMethod('imconnector.send.status.reading', payload);
    console.log(response.getData());
    ```

- PHP

    ```php
    $result = $b24Service->core->call(
        'imconnector.send.status.reading',
        [
            'CONNECTOR' => 'myconnector',
            'LINE' => 107,
            'MESSAGES' => [
                [
                    'im' => ['chat_id' => 323, 'message_id' => 85911],
                    'message' => ['id' => ['ext-msg-1007']],
                    'chat' => ['id' => 'channel-123'],
                ],
            ],
        ]
    );
    ```

- BX24.js

    ```js
    BX24.callMethod(
      'imconnector.send.status.reading',
      {
        CONNECTOR: 'myconnector',
        LINE: 107,
        MESSAGES: [
          {
            im: { chat_id: 323, message_id: 85911 },
            message: { id: ['ext-msg-1007'] },
            chat: { id: 'channel-123' },
          },
        ],
      },
      function(result) {
        console.log(result.data());
      }
    );
    ```

- PHP CRest

    ```php
    $result = CRest::call(
        'imconnector.send.status.reading',
        [
            'CONNECTOR' => 'myconnector',
            'LINE' => 107,
            'MESSAGES' => [
                [
                    'im' => ['chat_id' => 323, 'message_id' => 85911],
                    'message' => ['id' => ['ext-msg-1007']],
                    'chat' => ['id' => 'channel-123'],
                ],
            ],
        ]
    );
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
    "SUCCESS": true,
    "DATA": []
    },
    "time": {
    "start": 1738065600.11,
    "finish": 1738065600.21,
    "duration": 0.10,
    "processing": 0.04,
    "date_start": "2025-01-28T12:00:00+00:00",
    "date_finish": "2025-01-28T12:00:00+00:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **SUCCESS**
[`boolean`](../../data-types.md) | Returns `true` if the read status was successfully accepted and updated in Bitrix24 ||
|| **DATA**
[`array`](../../data-types.md) | Empty array upon successful processing ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "WRONG_AUTH_TYPE",
    "error_description": "Current authorization type is denied for this method Application context required"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `403` | `WRONG_AUTH_TYPE` | Current authorization type is denied for this method Application context required | Method called not in the context of an OAuth application ||
|| `400` | `ERROR_ARGUMENT` | Argument 'CONNECTOR' is null or empty | `CONNECTOR` not provided ||
|| `400` | `ERROR_ARGUMENT` | Argument 'LINE' is null or empty | `LINE` not provided ||
|| `400` | `ERROR_ARGUMENT` | Argument 'MESSAGES' is null or empty | `MESSAGES` not provided ||
|| `400` | `IMCONNECTOR_NO_CORRECT_PROVIDER` | Unable to find a suitable provider for the connector | Failed to initialize provider for the connector ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./imconnector-register.md)
- [{#T}](./imconnector-activate.md)
- [{#T}](./imconnector-status.md)
- [{#T}](./imconnector-connector-data-set.md)
- [{#T}](./imconnector-list.md)
- [{#T}](./imconnector-unregister.md)
- [{#T}](./imconnector-send-messages.md)
- [{#T}](./imconnector-update-messages.md)
- [{#T}](./imconnector-delete-messages.md)
- [{#T}](./imconnector-send-status-delivery.md)
- [{#T}](./imconnector-chat-name-set.md)