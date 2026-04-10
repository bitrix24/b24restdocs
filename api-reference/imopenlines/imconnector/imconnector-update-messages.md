# Update Sent Messages with imconnector.update.messages

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`imopenlines`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `imconnector.update.messages` updates the data of previously sent messages from an external system in an open line.

{% note info "" %}

The method works only in the context of an [application](../../../settings/app-installation/index.md).

{% endnote %} 

The method parameters use values from the external system: user ID, chat ID, chat link, and its name in the application that registered the connector.

To update, pass the same `message.id` and `chat.id` that were used when sending messages with the [imconnector.send.messages](./imconnector-send-messages.md) method.

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
[`array`](../../data-types.md) | Array of messages to update. Each element of the array is a single message in the object format with three required blocks: `user`, `message`, `chat`.

The structure of the object is described in detail [below](#messages) ||
|#

### MESSAGES Parameter {#messages}

#|
|| **Name**
`type` | **Description** ||
|| **user**
[`object`](../../data-types.md) | Data of the user from the external system. 

The structure of the object is described in detail [below](#messages-user) ||
|| **message**
[`object`](../../data-types.md) | Data of the message from the external system.

The structure of the object is described in detail [below](#messages-message) ||
|| **chat**
[`object`](../../data-types.md) | Data of the chat from the external system.

The structure of the object is described in detail [below](#messages-chat) ||
|#

#### User Object {#messages-user}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`string`](../../data-types.md) | User ID in the external system ||
|| **last_name**
[`string`](../../data-types.md) | User's last name ||
|| **name**
[`string`](../../data-types.md) | User's first name ||
|| **picture**
[`object`](../../data-types.md) | User's avatar in the object format with a `url` field, for example `{"url":"https://example.com/u42.png"}` ||
|| **url**
[`string`](../../data-types.md) | Link to the user's profile in the external system ||
|| **gender**
[`string`](../../data-types.md) | User's gender. Supported values are `male` and `female` ||
|| **email**
[`string`](../../data-types.md) | User's email ||
|| **phone**
[`string`](../../data-types.md) | User's phone ||
|| **skip_phone_validate**
[`string`](../../data-types.md) | Disables phone validation before saving in CRM. Acceptable value is `Y`. If validation should be performed, do not pass this parameter ||
|#

#### Message Object {#messages-message}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../data-types.md) | ID of the message in the external system that needs to be updated ||
|| **date**
[`integer`](../../data-types.md) | Message time in Unix Timestamp in seconds ||
|| **text**
[`string`](../../data-types.md) | New text of the message. Pass either `text` or `files`

For acceptable formatting, refer to the article [Formatting](../../chats/messages/formatting.md) ||
|| **files**
[`array`](../../data-types.md) | Array of files. Each file is passed as an array like `array('url' => 'File link', 'name' => 'File name')` ||
|| **disable_crm**
[`string`](../../data-types.md) | Disables the CRM tracker for the message. Acceptable value is `Y` ||
|| **user_id**
[`integer`](../../data-types.md) | ID of the manager in Bitrix24. If `user_id` is passed, the message will be updated on behalf of this manager ||
|#

#### Chat Object {#messages-chat}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`string`](../../data-types.md) | ID of the chat or channel in the external system. It is recommended to pass the same value as in `DATA.ID` of the [imconnector.connector.data.set](./imconnector-connector-data-set.md) method ||
|| **name**
[`string`](../../data-types.md) | Name of the chat or channel. It is recommended to use the value from `DATA.NAME` ||
|| **url**
[`string`](../../data-types.md) | Link to the chat or channel in the external system. It is recommended to use the value from `DATA.URL` ||
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
            "user": {
              "id": "ext-user-42",
              "last_name": "Ivanov",
              "name": "Ivan",
              "picture": {"url": "https://example.com/u42.png"},
              "url": "https://example.com/users/42",
              "gender": "male",
              "email": "ivan@example.com",
              "phone": "+19990000000"
            },
            "message": {
              "id": "ext-msg-1001",
              "date": 1773266050,
              "text": "Good afternoon, we clarified the details"
            },
            "chat": {
              "id": "channel-123",
              "name": "Support Channel",
              "url": "https://example.com/chats/123"
            }
          }
        ],
        "auth": "**put_access_token_here**"
      }' \
      https://**put_your_bitrix24_address**/rest/imconnector.update.messages
    ```

- JS

    ```js
    const payload = {
        CONNECTOR: 'myconnector',
        LINE: 107,
        MESSAGES: [
            {
                user: {
                    id: 'ext-user-42',
                    last_name: 'Ivanov',
                    name: 'Ivan',
                    picture: { url: 'https://example.com/u42.png' },
                    url: 'https://example.com/users/42',
                    gender: 'male',
                    email: 'ivan@example.com',
                    phone: '+19990000000',
                },
                message: {
                    id: 'ext-msg-1001',
                    date: 1773266050,
                    text: 'Good afternoon, we clarified the details',
                },
                chat: {
                    id: 'channel-123',
                    name: 'Support Channel',
                    url: 'https://example.com/chats/123',
                },
            },
        ],
    };

    const response = await $b24.callMethod('imconnector.update.messages', payload);
    console.log(response.getData());
    ```

- PHP

    ```php
    $result = $b24Service->core->call(
        'imconnector.update.messages',
        [
            'CONNECTOR' => 'myconnector',
            'LINE' => 107,
            'MESSAGES' => [
                [
                    'user' => [
                          'id' => 'ext-user-42',
                          'last_name' => 'Ivanov',
                          'name' => 'Ivan',
                          'picture' => ['url' => 'https://example.com/u42.png'],
                          'url' => 'https://example.com/users/42',
                          'gender' => 'male',
                          'email' => 'ivan@example.com',
                          'phone' => '+19990000000',
                    ],
                    'message' => [
                          'id' => 'ext-msg-1001',
                          'date' => 1773266050,
                          'text' => 'Good afternoon, we clarified the details',
                    ],
                    'chat' => [
                          'id' => 'channel-123',
                          'name' => 'Support Channel',
                          'url' => 'https://example.com/chats/123',
                    ],
                ],
            ],
        ]
    );
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'imconnector.update.messages',
        {
            CONNECTOR: 'myconnector',
            LINE: 107,
            MESSAGES: [
                {
                    user: {
                        id: 'ext-user-42',
                        last_name: 'Ivanov',
                        name: 'Ivan',
                        picture: { url: 'https://example.com/u42.png' },
                        url: 'https://example.com/users/42',
                        gender: 'male',
                        email: 'ivan@example.com',
                        phone: '+19990000000',
                    },
                    message: {
                        id: 'ext-msg-1001',
                        date: 1773266050,
                        text: 'Good afternoon, we clarified the details',
                    },
                    chat: {
                        id: 'channel-123',
                        name: 'Support Channel',
                        url: 'https://example.com/chats/123',
                    },
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
        'imconnector.update.messages',
        [
            'CONNECTOR' => 'myconnector',
            'LINE' => 107,
            'MESSAGES' => [
                [
                    'user' => [
                        'id' => 'ext-user-42',
                        'last_name' => 'Ivanov',
                        'name' => 'Ivan',
                        'picture' => ['url' => 'https://example.com/u42.png'],
                        'url' => 'https://example.com/users/42',
                        'gender' => 'male',
                        'email' => 'ivan@example.com',
                        'phone' => '+19990000000',
                    ],
                    'message' => [
                        'id' => 'ext-msg-1001',
                        'date' => 1773266050,
                        'text' => 'Good afternoon, we clarified the details',
                    ],
                    'chat' => [
                        'id' => 'channel-123',
                        'name' => 'Support Channel',
                        'url' => 'https://example.com/chats/123',
                    ],
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
                        "id": "ext-msg-1001",
                        "date": {},
                        "text": "Good afternoon, we clarified the details"
                    },
                    "chat": {
                        "id": "channel-123",
                        "name": "Support Channel",
                        "description": "Link to the original post: https://example.com/chats/123"
                    },
                    "SUCCESS": true
                }
            ]
        }
    },
    "time": {
        "start": 1773322669,
        "finish": 1773322669.300999,
        "duration": 0.3009989261627197,
        "processing": 0,
        "date_start": "2026-03-12T16:37:49+02:00",
        "date_finish": "2026-03-12T16:37:49+02:00",
        "operating_reset_at": 1773323269,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **SUCCESS**
[`boolean`](../../data-types.md) | Returns `true` if the message was successfully changed ||
|| **DATA**
[`object`](../../data-types.md) | Data of the message processing. 

The structure of the object is described in detail [below](#result-data) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### DATA Object {#result-data}

#|
|| **Name**
`type` | **Description** ||
|| **RESULT**
[`array`](../../data-types.md) | Array of results for each element of `MESSAGES`.

The structure of the element is described in detail [below](#result-item) ||
|#

#### RESULT[] Object {#result-item}

#|
|| **Name**
`type` | **Description** ||
|| **user**
[`string`](../../data-types.md) | Internal user ID in Bitrix24 ||
|| **message**
[`object`](../../data-types.md) | Data of the message after processing [(detailed description)](#result-item-message) ||
|| **chat**
[`object`](../../data-types.md) | Data of the chat after processing [(detailed description)](#result-item-chat) ||
|| **extra**
[`object`](../../data-types.md) | Additional flags for message processing [(detailed description)](#result-item-extra) ||
|| **SUCCESS**
[`boolean`](../../data-types.md) | Indicates successful processing of the current array element ||
|| **ERRORS**
[`array`](../../data-types.md) | Array of error texts for the current element, returned when `SUCCESS = false` ||
|#

#### Message Object {#result-item-message}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../data-types.md) | External message ID ||
|| **date**
[`object`](../../data-types.md) | Date of the message after processing ||
|| **text**
[`string`](../../data-types.md) | Text of the message ||
|| **files**
[`array`](../../data-types.md) | Array of message files ||
|#

#### Chat Object {#result-item-chat}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../data-types.md) | ID of the chat or channel ||
|| **name**
[`string`](../../data-types.md) | Name of the chat or channel ||
|| **description**
[`string`](../../data-types.md) | Description of the chat, for example, a link to the original post ||
|#

#### Extra Object {#result-item-extra}

#|
|| **Name**
`type` | **Description** ||
|| **skip_phone_validate**
[`string`](../../data-types.md) | Indicates phone validation is disabled, returned with value `Y` ||
|| **disable_tracker**
[`string`](../../data-types.md) | Indicates the CRM tracker is disabled, returned with value `Y` ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "ERROR_ARGUMENT",
    "error_description": "Argument 'LINE' is null or empty"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `403` | `WRONG_AUTH_TYPE` | Current authorization type is denied for this method Application context required | Method called not in the context of an application OAuth ||
|| `400` | `ERROR_ARGUMENT` | Argument 'CONNECTOR' is null or empty | `CONNECTOR` not provided ||
|| `400` | `ERROR_ARGUMENT` | Argument 'LINE' is null or empty | `LINE` not provided ||
|| `400` | `ERROR_ARGUMENT` | Argument 'MESSAGES' is null or empty | `MESSAGES` not provided ||
|| `400` | `NOT_ACTIVE_LINE` | The line with this ID is inactive or does not exist | An inactive `LINE` was provided ||
|| `400` | `IMCONNECTOR_NO_CORRECT_PROVIDER` | Failed to find a suitable provider for the connector | Unable to initialize provider for the connector ||
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
- [{#T}](./imconnector-delete-messages.md)
- [{#T}](./imconnector-send-status-delivery.md)
- [{#T}](./imconnector-chat-name-set.md)