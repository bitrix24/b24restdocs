# Delete Sent Messages imconnector.delete.messages

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`imopenlines`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `imconnector.delete.messages` removes messages from an open line that were sent by an external system.

{% note info "" %}

The method works only in the context of an [application](../../../settings/app-installation/index.md).

{% endnote %} 

The method parameters use values from the external system: user ID, message ID, and chat or channel ID.

To delete messages, provide `message.id` and `chat.id` that were used when sending messages via the [imconnector.send.messages](./imconnector-send-messages.md) method.

## Method Parameters

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CONNECTOR***
[`string`](../../data-types.md) | String code of the connector specified in the `ID` parameter when calling [imconnector.register](./imconnector-register.md) ||
|| **LINE***
[`integer`](../../data-types.md) | Identifier of the open line. 

The identifier can be obtained using the [imopenlines.config.get](../openlines/imopenlines-config-get.md) and [imopenlines.config.list.get](../openlines/imopenlines-config-list-get.md) methods ||
|| **MESSAGES***
[`array`](../../data-types.md) | Array of messages to delete. Each element of the array is a message object with three required blocks: `user`, `message`, `chat`. The structure of the object is described in detail [below](#messages) ||
|#

### MESSAGES Parameter {#messages}

#|
|| **Name**
`type` | **Description** ||
|| **user**
[`object`](../../data-types.md) | User data from the external system.

The structure of the object is described in detail [below](#messages-user) ||
|| **message**
[`object`](../../data-types.md) | Message data from the external system.

The structure of the object is described in detail [below](#messages-message) ||
|| **chat**
[`object`](../../data-types.md) | Chat or channel data from the external system.

The structure of the object is described in detail [below](#messages-chat) ||
|#

#### User Object {#messages-user}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`string`](../../data-types.md) | User ID in the external system. 

It is recommended to provide the same value used in `user.id` of the [imconnector.send.messages](./imconnector-send-messages.md) method ||
|#

#### Message Object {#messages-message}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`string`](../../data-types.md) | Message ID in the external system that needs to be deleted. 

It is recommended to provide the same value used in `message.id` of the [imconnector.send.messages](./imconnector-send-messages.md) method ||
|#

#### Chat Object {#messages-chat}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`string`](../../data-types.md) | Chat or channel ID in the external system. 

It is recommended to provide the same value as in `chat.id` of the [imconnector.send.messages](./imconnector-send-messages.md) method ||
|#

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

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
            "user": {"id": "ext-user-42"},
            "message": {"id": "ext-msg-1001"},
            "chat": {"id": "channel-123"}
          }
        ],
        "auth": "**put_access_token_here**"
      }' \
      https://**put_your_bitrix24_address**/rest/imconnector.delete.messages
    ```

- JS

    ```js
    const payload = {
      CONNECTOR: 'myconnector',
      LINE: 107,
      MESSAGES: [
        {
          user: { id: 'ext-user-42' },
          message: { id: 'ext-msg-1001' },
          chat: { id: 'channel-123' },
        },
      ],
    };

    const response = await $b24.callMethod('imconnector.delete.messages', payload);
    console.log(response.getData());
    ```

- PHP

    ```php
    $result = $b24Service->core->call(
        'imconnector.delete.messages',
        [
            'CONNECTOR' => 'myconnector',
            'LINE' => 107,
            'MESSAGES' => [
                [
                    'user' => ['id' => 'ext-user-42'],
                    'message' => ['id' => 'ext-msg-1001'],
                    'chat' => ['id' => 'channel-123'],
                ],
            ],
        ]
    );
    ```

- BX24.js

    ```js
    BX24.callMethod(
      'imconnector.delete.messages',
      {
        CONNECTOR: 'myconnector',
        LINE: 107,
        MESSAGES: [
          {
            user: { id: 'ext-user-42' },
            message: { id: 'ext-msg-1001' },
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
        'imconnector.delete.messages',
        [
            'CONNECTOR' => 'myconnector',
            'LINE' => 107,
            'MESSAGES' => [
                [
                    'user' => ['id' => 'ext-user-42'],
                    'message' => ['id' => 'ext-msg-1001'],
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
        "DATA": {
            "RESULT": [
                {
                    "user": "585",
                    "message": {
                        "id": "ext-msg-1001"
                    },
                    "chat": {
                        "id": "channel-123"
                    },
                    "SUCCESS": true
                }
            ]
        }
    },
    "time": {
        "start": 1773267564,
        "finish": 1773267565.070724,
        "duration": 1.0707240104675293,
        "processing": 1,
        "date_start": "2026-03-11T14:19:24+01:00",
        "date_finish": "2026-03-11T14:19:25+01:00",
        "operating_reset_at": 1773268164,
        "operating": 0.113616943359375
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **SUCCESS**
[`boolean`](../../data-types.md) | Returns `true` if the message deletion was successful ||
|| **DATA**
[`object`](../../data-types.md) | Data regarding the message processing. 

The structure of the object is described in detail [below](#result-data) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### DATA Object {#result-data}

#|
|| **Name**
`type` | **Description** ||
|| **RESULT**
[`array`](../../data-types.md) | Array of results for each element of `MESSAGES`

The structure of each element is described in detail [below](#result-item) ||
|#

#### RESULT[] Object {#result-item}

#|
|| **Name**
`type` | **Description** ||
|| **user**
[`string`](../../data-types.md) | Internal user ID in Bitrix24 ||
|| **message**
[`object`](../../data-types.md) | Data of the deleted message [(detailed description)](#result-item-message) ||
|| **chat**
[`object`](../../data-types.md) | Data of the chat or channel [(detailed description)](#result-item-chat) ||
|| **SUCCESS**
[`boolean`](../../data-types.md) | Indicator of successful processing of the current array element ||
|| **ERRORS**
[`array`](../../data-types.md) | Array of error texts for the current element, returned when `SUCCESS = false` ||
|#

#### Message Object {#result-item-message}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../data-types.md) | Message ID in the external system that was passed in `MESSAGES[].message.id` ||
|| **date**
[`integer`](../../data-types.md) | Message time in Unix Timestamp, if this field was provided in the input data ||
|| **text**
[`string`](../../data-types.md) | Text of the message, if this field was provided in the input data ||
|| **files**
[`array`](../../data-types.md) | Array of message files, if this field was provided in the input data ||
|#

#### Chat Object {#result-item-chat}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../data-types.md) | Chat or channel ID in the external system that was passed in `MESSAGES[].chat.id` ||
|| **description**
[`string`](../../data-types.md) | Description of the chat. This field is returned if a `url` link was provided in `MESSAGES[].chat` ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "ERROR_ARGUMENT",
    "error_description": "Argument 'CONNECTOR' is null or empty"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `403` | `WRONG_AUTH_TYPE` | Current authorization type is denied for this method Application context required | Method called outside of the application context OAuth ||
|| `400` | `ERROR_ARGUMENT` | Argument 'CONNECTOR' is null or empty | `CONNECTOR` not provided ||
|| `400` | `ERROR_ARGUMENT` | Argument 'LINE' is null or empty | `LINE` not provided ||
|| `400` | `ERROR_ARGUMENT` | Argument 'MESSAGES' is null or empty | `MESSAGES` not provided ||
|| `400` | `NOT_ACTIVE_LINE` | Line with such ID is inactive or does not exist | Inactive `LINE` provided ||
|| `400` | `IMCONNECTOR_NO_CORRECT_PROVIDER` | Could not find a suitable provider for the connector | Failed to initialize provider for the connector ||
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
- [{#T}](./imconnector-send-status-delivery.md)
- [{#T}](./imconnector-chat-name-set.md)