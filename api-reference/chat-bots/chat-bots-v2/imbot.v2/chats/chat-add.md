# Create a Group Chat imbot.v2.Chat.add

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`imbot`](../../../../scopes/permissions.md)
>
> Who can execute the method: owner of the registered bot

The method `imbot.v2.Chat.add` creates a new group chat on behalf of the bot. If the owner is not specified, the bot becomes the owner.

## Method Parameters

{% include [Note on parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **botId***
[`integer`](../../../../data-types.md) | Bot ID ||
|| **botToken**
[`string`](../../../../data-types.md) | Unique authorization token for the bot. Required for webhook authorization, not needed for OAuth.

Pass the same botToken that was specified during the chat bot registration ||
|| **fields**
[`object`](../../../../data-types.md) | Properties of the chat being created. The structure of the object is described [below](#fields) ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`Type` | **Description** ||
|| **title**
[`string`](../../../../data-types.md) | Chat title ||
|| **description**
[`string`](../../../../data-types.md) | Chat description ||
|| **color**
[`string`](../../../../data-types.md) | Chat color — [available colors](#available-colors).

If not specified or incorrect, it will be assigned automatically ||
|| **avatar**
[`file`](../../../../data-types.md) | Chat avatar in [Base64](../../../../files/how-to-upload-files.md) format ||
|| **userIds**
[`integer[]`](../../../../data-types.md) | Array of participant IDs ||
|| **ownerId**
[`integer`](../../../../data-types.md) | ID of the chat owner. If not specified, the bot becomes the owner ||
|| **message**
[`string`](../../../../data-types.md) | First message in the chat ||
|#

### Available Colors {#available-colors}

#|
|| **Code** | **HEX** ||
|| `red` | `#df532d` ||
|| `green` | `#64a513` ||
|| `mint` | `#4ba984` ||
|| `lightBlue` | `#4ba5c3` ||
|| `darkBlue` | `#3e99ce` ||
|| `purple` | `#8474c8` ||
|| `aqua` | `#1eb4aa` ||
|| `pink` | `#f76187` ||
|| `lime` | `#58cc47` ||
|| `brown` | `#ab7761` ||
|| `azure` | `#29619b` ||
|| `khaki` | `#728f7a` ||
|| `sand` | `#ba9c7b` ||
|| `marengo` | `#556574` ||
|| `gray` | `#909090` ||
|| `graphite` | `#5e5f5e` ||
|#

## Code Examples

{% include [Note on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"botId":456,"botToken":"my_bot_token","fields":{"title":"Support Chat","color":"mint","userIds":[1,2]}}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imbot.v2.Chat.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"botId":456,"fields":{"title":"Support Chat","color":"mint","userIds":[1,2]},"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/imbot.v2.Chat.add
    ```

- JS

    ```js
    try {
      const response = await $b24.callMethod('imbot.v2.Chat.add', {
        botId: 456,
        fields: {
          title: 'Support Chat',
          color: 'mint',
          userIds: [1, 2],
        },
      });

      const { result } = response.getData();
      console.log('result:', result);
    } catch (error) {
      console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'imbot.v2.Chat.add',
                [
                    'botId' => 456,
                    'fields' => [
                        'title' => 'Support Chat',
                        'color' => 'mint',
                        'userIds' => [1, 2],
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'result: '. print_r($result, true);
    } catch (Throwable $exception) {
        error_log($exception->getMessage());
        echo 'Error: '. $exception->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'imbot.v2.Chat.add',
        {
            botId: 456,
            fields: {
                title: 'Support Chat',
                color: 'mint',
                userIds: [1, 2],
            },
        },
        function(result) {
            if (result.error()) {
                console.error(result.error().ex);
            } else {
                console.log('Chat ID:', result.data().chat.id);
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'imbot.v2.Chat.add',
        [
            'botId' => 456,
            'fields' => [
                'title' => 'Support Chat',
                'color' => 'mint',
                'userIds' => [1, 2],
            ],
        ]
    );

    if (!empty($result['error'])) {
        echo 'Error: '. $result['error_description'];
    } else {
        echo 'Chat ID: '. $result['result']['chat']['id'];
    }
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "chat": {
            "id": 5,
            "dialogId": "chat5",
            "name": "Support Chat",
            "description": "",
            "type": "chat",
            "messageType": "C",
            "owner": 456,
            "color": "#4ba984",
            "avatar": "",
            "extranet": false,
            "containsCollaber": false,
            "entityType": "",
            "entityId": "",
            "entityData1": "",
            "entityData2": "",
            "entityData3": "",
            "entityLink": {},
            "diskFolderId": 42,
            "role": "owner",
            "permissions": {},
            "muteList": [],
            "parentChatId": null,
            "parentMessageId": null,
            "isNew": true,
            "textFieldEnabled": "Y",
            "backgroundId": null,
            "dateCreate": "2025-01-15T10:00:00+02:00",
            "lastMessageId": 789,
            "lastMessageViews": "{}",
            "lastId": null,
            "managerList": [],
            "markedId": null,
            "messageCount": 1,
            "public": "",
            "unreadId": null,
            "userCounter": 3
        },
        "users": [
            {
                "id": 1,
                "active": true,
                "name": "John Smith",
                "firstName": "John",
                "lastName": "Smith",
                "workPosition": "Developer",
                "color": "#ab7761",
                "avatar": "",
                "gender": "M",
                "birthday": "15-03",
                "extranet": false,
                "bot": false,
                "connector": false,
                "externalAuthId": "default",
                "status": "online",
                "idle": false,
                "lastActivityDate": "2025-01-15T14:25:00+02:00",
                "absent": false,
                "departments": [7],
                "phones": false,
                "type": "employee"
            },
            {
                "id": 2,
                "active": true,
                "name": "Anna Davis",
                "firstName": "Anna",
                "lastName": "Davis",
                "workPosition": "Manager",
                "color": "#5b7e91",
                "avatar": "",
                "gender": "F",
                "birthday": "22-08",
                "extranet": false,
                "bot": false,
                "connector": false,
                "externalAuthId": "default",
                "status": "online",
                "idle": false,
                "lastActivityDate": "2025-01-15T14:20:00+02:00",
                "absent": false,
                "departments": [12],
                "phones": false,
                "type": "employee"
            }
        ]
    },
    "time": {
        "start": 1728626400.123,
        "finish": 1728626400.234,
        "duration": 0.111,
        "processing": 0.045,
        "date_start": "2024-10-11T10:00:00+02:00",
        "date_finish": "2024-10-11T10:00:00+02:00"
    }
}
```

## Returned Data

#|
|| **Name**
`Type` | **Description** ||
|| **result**
[`object`](../../../../data-types.md) | Result of chat creation ||
|| **result.chat**
[`Chat`](../../entities.md#chat) | Object of the created chat [(detailed description)](#chat-object) ||
|| **result.users**
[`User[]`](../../entities.md#user) | Array of chat participants [(detailed description)](#user-object) ||
|| **time**
[`time`](../../../../data-types.md#time) | Information about the request execution time ||
|#

### Fields of the Chat Object {#chat-object}

#|
|| **Field**
`Type` | **Description** ||
|| **id**
[`integer`](../../../../data-types.md) | Unique identifier of the chat ||
|| **dialogId**
[`string`](../../../../data-types.md) | Identifier of the dialog: `chat5` for group chats, `123` for personal chats ||
|| **name**
[`string`](../../../../data-types.md) | Chat title ||
|| **description**
[`string`](../../../../data-types.md) | Chat description ||
|| **type**
[`string`](../../../../data-types.md) | Type of chat: `chat`, `open`, `channel`, and others ||
|| **owner**
[`integer`](../../../../data-types.md) | ID of the chat owner ||
|| **color**
[`string\|null`](../../../../data-types.md) | Chat color in HEX format ||
|| **avatar**
[`string`](../../../../data-types.md) | URL of the chat avatar. An empty string if not set ||
|| **role**
[`string`](../../../../data-types.md) | Role of the current user: `owner`, `manager`, `member`, `guest`, `none` ||
|| **dateCreate**
[`string\|null`](../../../../data-types.md) | Date of chat creation in ISO 8601 format ||
|| **lastMessageId**
[`integer\|null`](../../../../data-types.md) | ID of the last message ||
|| **muteList**
[`array`](../../../../data-types.md) | List of user IDs who have disabled notifications ||
|| **managerList**
[`array`](../../../../data-types.md) | Array of chat managers' IDs ||
|#

Complete description of all fields is available on the page [Objects and Fields — Chat](../../entities.md#chat).

### Fields of the User Object {#user-object}

#|
|| **Field**
`Type` | **Description** ||
|| **id**
[`integer`](../../../../data-types.md) | Unique identifier of the user ||
|| **active**
[`boolean`](../../../../data-types.md) | Is the user active in the system ||
|| **name**
[`string`](../../../../data-types.md) | Full name ||
|| **type**
[`string`](../../../../data-types.md) | Type of user: `employee`, `extranet`, `bot`, and others ||
|#

Complete description of all fields is available on the page [Objects and Fields — User](../../entities.md#user).

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "BOT_NOT_FOUND",
    "error_description": "Bot not found"
}
```

{% include notitle [Error Handling](../../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `BOT_TOKEN_NOT_SPECIFIED` | Bot token is not specified | `botToken` is not specified. Required for webhook authorization ||
|| `BOT_ID_REQUIRED` | Bot ID is required | `botId` is not specified ||
|| `BOT_NOT_FOUND` | Bot not found | Bot not found ||
|| `BOT_OWNERSHIP_ERROR` | Bot is registered by another application | Bot is registered by another application ||
|#

{% include [System Errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [API Change Log imbot.v2](../../change-log.md)
- [{#T}](./chat-get.md)
- [{#T}](./chat-update.md)
- [{#T}](./chat-user-add.md)
- [{#T}](./chat-leave.md)