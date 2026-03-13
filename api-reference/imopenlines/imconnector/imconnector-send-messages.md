# Send Messages to Bitrix24 imconnector.send.messages

> Scope: [`imopenlines`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `imconnector.send.messages` accepts messages from an external system and forwards them to an open line in Bitrix24 via a custom connector.

The method parameters utilize values from the external system: user ID, chat ID, chat link, and its name in the application that registered the connector.

Upon execution, the method returns the chat ID and the open line dialog ID created in Bitrix24.

{% note info "" %}

The method works only in the context of an [application](../../../settings/app-installation/index.md).

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CONNECTOR***
[`string`](../../data-types.md) | The string code of the connector specified in the `ID` parameter when calling [imconnector.register](./imconnector-register.md) ||
|| **LINE***
[`integer`](../../data-types.md) | The ID of the open line.

The ID can be obtained using the methods [imopenlines.config.get](../openlines/imopenlines-config-get.md) and [imopenlines.config.list.get](../openlines/imopenlines-config-list-get.md) ||
|| **MESSAGES***
[`array`](../../data-types.md) | An array of messages. Each element of the array is a single message in the object format with three required blocks: `user`, `message`, `chat`.

The structure of the object is described in detail [below](#messages) ||
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
[`object`](../../data-types.md) | Chat data from the external system.

The structure of the object is described in detail [below](#messages-chat) ||
|#

#### User Object {#messages-user}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`string`](../../data-types.md) | User ID in the external system. The value is generated on the side of the external system ||
|| **last_name**
[`string`](../../data-types.md) | User's last name ||
|| **name**
[`string`](../../data-types.md) | User's first name ||
|| **picture**
[`object`](../../data-types.md) | User's avatar. Pass as an object with the `url` field, for example, `{"url":"https://example.com/u42.png"}`. The link must be public ||
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

The `name` and `last_name` fields can contain letters, spaces, hyphens, and apostrophes. Numbers and other special characters are not allowed. The maximum length for `name` and `last_name` is 25 characters. The language and case can be any.

{% note info "" %}

The `skip_phone_validate` parameter is recommended to be used only in rare cases when the number fails the built-in validation. This is a workaround to bypass the phone validator's limitations. In a typical scenario, pass the phone without this parameter.

{% endnote %}

#### Message Object {#messages-message}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../data-types.md) | Message ID in the external system. The value is generated on the side of the external system ||
|| **date**
[`integer`](../../data-types.md) | Message time in Unix Timestamp in seconds ||
|| **text**
[`string`](../../data-types.md) | Message text. You must pass either the `text` or `files` element.

For acceptable formatting, refer to the article [Formatting](../../chats/messages/index.md) ||
|| **files**
[`array`](../../data-types.md) | An array of files. Each file is passed as an array of the form `array('url' => 'File link', 'name' => 'File name')`. The `url` link must be accessible from Bitrix24.

The format of the transmitted file is not restricted. In chat, the attachment can be displayed as an image for types: `jpe`, `jpg`, `jpeg`, `png`, `webp`, `gif`, `bmp` ||
|| **disable_crm**
[`string`](../../data-types.md) | Disables the CRM tracker for the message. Acceptable value is `Y`. If the CRM tracker should work, do not pass this parameter ||
|| **user_id**
[`integer`](../../data-types.md) | The ID of the manager in Bitrix24. If `user_id` is passed, the message will be sent on behalf of this manager ||
|#

#### Chat Object {#messages-chat}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`string`](../../data-types.md) | Chat or channel ID in the external system.

It is recommended to pass the same value as in `DATA.ID` of the method [imconnector.connector.data.set](./imconnector-connector-data-set.md) ||
|| **name**
[`string`](../../data-types.md) | Chat or channel name.

It is recommended to use the value from `DATA.NAME` ||
|| **url**
[`string`](../../data-types.md) | Link to the chat or channel in the external system.

It is recommended to use the value from `DATA.URL` ||
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
            "user": {
              "id": "ext-user-42",
              "last_name": "Smith",
              "name": "John",
              "picture": {"url": "https://example.com/u42.png"},
              "url": "https://example.com/users/42",
              "gender": "male",
              "email": "john@example.com",
              "phone": "+19990000000",
              "skip_phone_validate": "Y"
            },
            "message": {
              "id": "ext-msg-1001",
              "date": 1773265993,
              "text": "Good afternoon",
              "files": [
                {"url": "https://example.com/files/spec.pdf", "name": "spec.pdf"}
              ],
              "disable_crm": "Y"
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
      https://**put_your_bitrix24_address**/rest/imconnector.send.messages
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
            last_name: 'Smith',
            name: 'John',
            picture: { url: 'https://example.com/u42.png' },
            url: 'https://example.com/users/42',
            gender: 'male',
            email: 'john@example.com',
            phone: '+19990000000',
            skip_phone_validate: 'Y',
          },
          message: {
            id: 'ext-msg-1001',
            date: 1773265993,
            text: 'Good afternoon',
            files: [{ url: 'https://example.com/files/spec.pdf', name: 'spec.pdf' }],
            disable_crm: 'Y',
          },
          chat: {
            id: 'channel-123',
            name: 'Support Channel',
            url: 'https://example.com/chats/123',
          },
        },
      ],
    };

    const response = await $b24.callMethod('imconnector.send.messages', payload);
    console.log(response.getData());
    ```

- PHP

    ```php
    $result = $b24Service->core->call(
        'imconnector.send.messages',
        [
            'CONNECTOR' => 'myconnector',
            'LINE' => 107,
            'MESSAGES' => [
                [
                    'user' => [
                        'id' => 'ext-user-42',
                        'last_name' => 'Smith',
                        'name' => 'John',
                        'picture' => ['url' => 'https://example.com/u42.png'],
                        'url' => 'https://example.com/users/42',
                        'gender' => 'male',
                        'email' => 'john@example.com',
                        'phone' => '+19990000000',
                        'skip_phone_validate' => 'Y',
                    ],
                    'message' => [
                        'id' => 'ext-msg-1001',
                        'date' => 1773265993,
                        'text' => 'Good afternoon',
                        'files' => [
                            ['url' => 'https://example.com/files/spec.pdf', 'name' => 'spec.pdf'],
                        ],
                        'disable_crm' => 'Y',
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
      'imconnector.send.messages',
      {
        CONNECTOR: 'myconnector',
        LINE: 107,
        MESSAGES: [
          {
            user: {
              id: 'ext-user-42',
              last_name: 'Smith',
              name: 'John',
              picture: { url: 'https://example.com/u42.png' },
              url: 'https://example.com/users/42',
              gender: 'male',
              email: 'john@example.com',
              phone: '+19990000000',
              skip_phone_validate: 'Y',
            },
            message: {
              id: 'ext-msg-1001',
              date: 1773265993,
              text: 'Good afternoon',
              files: [{ url: 'https://example.com/files/spec.pdf', name: 'spec.pdf' }],
              disable_crm: 'Y',
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
        'imconnector.send.messages',
        [
            'CONNECTOR' => 'myconnector',
            'LINE' => 107,
            'MESSAGES' => [
                [
                    'user' => [
                        'id' => 'ext-user-42',
                        'last_name' => 'Smith',
                        'name' => 'John',
                        'picture' => ['url' => 'https://example.com/u42.png'],
                        'url' => 'https://example.com/users/42',
                        'gender' => 'male',
                        'email' => 'john@example.com',
                        'phone' => '+19990000000',
                        'skip_phone_validate' => 'Y',
                    ],
                    'message' => [
                        'id' => 'ext-msg-1001',
                        'date' => 1773265993,
                        'text' => 'Good afternoon',
                        'files' => [
                            ['url' => 'https://example.com/files/spec.pdf', 'name' => 'spec.pdf'],
                        ],
                        'disable_crm' => 'Y',
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
                        "text": "Good afternoon",
                        "files": []
                    },
                    "chat": {
                        "id": "channel-123",
                        "name": "Support Channel",
                        "description": "Link to the original post: https://example.com/chats/123"
                    },
                    "extra": {
                        "skip_phone_validate": "Y",
                        "disable_tracker": "Y"
                    },
                    "SUCCESS": true,
                    "session": {
                        "ID": "323",
                        "CHAT_ID": "1767"
                    }
                }
            ]
        }
    },
    "time": {
        "start": 1773265993,
        "finish": 1773265994.487149,
        "duration": 1.4871490001678467,
        "processing": 1,
        "date_start": "2026-03-11T13:53:13+01:00",
        "date_finish": "2026-03-11T13:53:14+01:00",
        "operating_reset_at": 1773266593,
        "operating": 1.1916680335998535
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **SUCCESS**
[`boolean`](../../data-types.md) | Returns `true` if the message was sent successfully ||
|| **DATA**
[`object`](../../data-types.md) | Data containing information about the sent messages.

The structure of the object is described in detail [below](#result-data) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### DATA Object {#result-data}

#|
|| **Name**
`type` | **Description** ||
|| **RESULT**
[`array`](../../data-types.md) | An array of results for each element of `MESSAGES`.

The structure of the element is described in detail [below](#result-item) ||
|#

#### RESULT[] Object {#result-item}

#|
|| **Name**
`type` | **Description** ||
|| **user**
[`string`](../../data-types.md) | Internal user ID in Bitrix24 ||
|| **message**
[`object`](../../data-types.md) | Message data after processing [(detailed description)](#result-item-message) ||
|| **chat**
[`object`](../../data-types.md) | Chat data after processing [(detailed description)](#result-item-chat) ||
|| **extra**
[`object`](../../data-types.md) | Additional flags for message processing [(detailed description)](#result-item-extra) ||
|| **SUCCESS**
[`boolean`](../../data-types.md) | Indicates successful processing of the current array element ||
|| **ERRORS**
[`array`](../../data-types.md) | An array of error texts for the current element, returned when `SUCCESS = false` ||
|| **session**
[`object`](../../data-types.md) | Information about the open line session, returned if available [(detailed description)](#result-item-session) ||
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
[`string`](../../data-types.md) | Message text ||
|| **files**
[`array`](../../data-types.md) | An array of message files ||
|#

#### Chat Object {#result-item-chat}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../data-types.md) | Chat or channel ID ||
|| **name**
[`string`](../../data-types.md) | Chat or channel name ||
|| **description**
[`string`](../../data-types.md) | Description of the chat, such as a link to the original post ||
|#

#### Extra Object {#result-item-extra}

#|
|| **Name**
`type` | **Description** ||
|| **skip_phone_validate**
[`string`](../../data-types.md) | Indicates phone validation is disabled, returned when the value is `Y` ||
|| **disable_tracker**
[`string`](../../data-types.md) | Indicates the CRM tracker is disabled, returned when the value is `Y` ||
|#

#### Session Object {#result-item-session}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`string`](../../data-types.md) | Open line session ID ||
|| **CHAT_ID**
[`string`](../../data-types.md) | Chat ID in Bitrix24 ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "ERROR_ARGUMENT",
    "error_description": "Argument 'MESSAGES' is null or empty"
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
|| `400` | `ERROR_ARGUMENT` | The value of an argument 'MESSAGES' must be of type array | `MESSAGES` not passed as an array ||
|| `400` | `ERROR_ARGUMENT` | The MESSAGES parameter must be an array of messages (arrays) | Elements of `MESSAGES` not passed as arrays ||
|| `400` | `ERROR_ARGUMENT` | The incorrect structure of a message inside MESSAGES parameter | The `MESSAGES` element is missing `user`, `message`, or `chat` ||
|| `400` | `NOT_ACTIVE_LINE` | The line with this ID is inactive or does not exist | An inactive `LINE` was passed ||
|| `400` | `IMCONNECTOR_NO_CORRECT_PROVIDER` | Failed to find a suitable provider for the connector | Unable to initialize the provider for the connector ||
|| `400` | `IMCONNECTOR_NOT_SPECIFIED_CORRECT_COMMAND` | No valid command specified | Unable to determine the command for processing incoming data ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./imconnector-register.md)
- [{#T}](./imconnector-activate.md)
- [{#T}](./imconnector-status.md)
- [{#T}](./imconnector-connector-data-set.md)
- [{#T}](./imconnector-list.md)
- [{#T}](./imconnector-unregister.md)
- [{#T}](./imconnector-update-messages.md)
- [{#T}](./imconnector-delete-messages.md)
- [{#T}](./imconnector-send-status-delivery.md)
- [{#T}](./imconnector-chat-name-set.md)
- [{#T}](../../../tutorials/openlines/example-connector.md)