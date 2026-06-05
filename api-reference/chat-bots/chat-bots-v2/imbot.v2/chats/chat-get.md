# Get Information About the Chat imbot.v2.Chat.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`imbot`](../../../../scopes/permissions.md)
>
> Who can execute the method: owner of the registered bot

The method `imbot.v2.Chat.get` returns information about the chat. The bot must be a participant in the chat.

## Method Parameters

{% include [Parameter Note](../../../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **botId***
[`integer`](../../../../data-types.md) | Bot ID ||
|| **botToken**
[`string`](../../../../data-types.md) | Unique authorization token for the bot. Required for webhook authorization, not needed for OAuth.

Pass the same botToken that was specified during the registration of the chatbot ||
|| **dialogId***
[`string`](../../../../data-types.md) | Dialog ID. For group chats — `chat{chatId}`, for personal chats — `{userId}` ||
|#

## Code Examples

{% include [Examples Note](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"botId":456,"botToken":"my_bot_token","dialogId":"chat5"}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imbot.v2.Chat.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"botId":456,"dialogId":"chat5","auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/imbot.v2.Chat.get
    ```

- JS

    ```js
    try {
      const response = await $b24.callMethod('imbot.v2.Chat.get', {
        botId: 456,
        dialogId: 'chat5',
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
                'imbot.v2.Chat.get',
                [
                    'botId' => 456,
                    'dialogId' => 'chat5',
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
        'imbot.v2.Chat.get',
        {
            botId: 456,
            dialogId: 'chat5',
        },
        function(result) {
            if (result.error()) {
                console.error(result.error().ex);
            } else {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'imbot.v2.Chat.get',
        [
            'botId' => 456,
            'dialogId' => 'chat5',
        ]
    );

    if (!empty($result['error'])) {
        echo 'Error: '. $result['error_description'];
    } else {
        echo 'Chat name: '. $result['result']['chat']['name'];
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
            "isNew": false,
            "textFieldEnabled": "Y",
            "backgroundId": null,
            "dateCreate": "2025-01-15T10:00:00+01:00",
            "lastMessageId": 789,
            "lastMessageViews": "{}",
            "lastId": 789,
            "managerList": [],
            "markedId": null,
            "messageCount": 15,
            "public": "",
            "unreadId": null,
            "userCounter": 3
        }
    },
    "time": {
        "start": 1728626400.123,
        "finish": 1728626400.234,
        "duration": 0.111,
        "processing": 0.045,
        "date_start": "2024-10-11T10:00:00+01:00",
        "date_finish": "2024-10-11T10:00:00+01:00"
    }
}
```

## Returned Data

#|
|| **Name**
`Type` | **Description** ||
|| **result**
[`object`](../../../../data-types.md) | Result of the request ||
|| **result.chat**
[`Chat`](../../entities.md#chat) | Chat object [(detailed description)](#chat-object) ||
|| **time**
[`time`](../../../../data-types.md#time) | Information about the request execution time ||
|#

### Chat Object Fields {#chat-object}

#|
|| **Field**
`Type` | **Description** ||
|| **id**
[`integer`](../../../../data-types.md) | Unique identifier of the chat ||
|| **dialogId**
[`string`](../../../../data-types.md) | Dialog identifier: `chat5` for group chats, `123` for personal chats ||
|| **name**
[`string`](../../../../data-types.md) | Name of the chat ||
|| **description**
[`string`](../../../../data-types.md) | Description of the chat ||
|| **type**
[`string`](../../../../data-types.md) | Type of chat: `chat`, `open`, `channel`, and others ||
|| **owner**
[`integer`](../../../../data-types.md) | ID of the chat owner ||
|| **color**
[`string\|null`](../../../../data-types.md) | Color of the chat in HEX format ||
|| **avatar**
[`string`](../../../../data-types.md) | URL of the chat avatar. Empty string if not set ||
|| **role**
[`string`](../../../../data-types.md) | Role of the current user: `owner`, `manager`, `member`, `guest`, `none` ||
|| **dateCreate**
[`string\|null`](../../../../data-types.md) | Creation date of the chat in ISO 8601 format ||
|| **lastMessageId**
[`integer\|null`](../../../../data-types.md) | ID of the last message ||
|| **muteList**
[`array`](../../../../data-types.md) | List of user IDs who have disabled notifications ||
|| **managerList**
[`array`](../../../../data-types.md) | Array of chat manager IDs ||
|#

Full description of all fields can be found on the page [Objects and Fields — Chat](../../entities.md#chat).

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "ACCESS_DENIED",
    "error_description": "Access denied"
}
```

{% include notitle [Error Handling](../../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `BOT_TOKEN_NOT_SPECIFIED` | Bot token is not specified | `botToken` not specified. Required for webhook authorization ||
|| `BOT_ID_REQUIRED` | Bot ID is required | `botId` not specified ||
|| `BOT_NOT_FOUND` | Bot not found | Bot not found ||
|| `BOT_OWNERSHIP_ERROR` | Bot is registered by another application | Bot registered by another application ||
|| `ACCESS_DENIED` | Access denied | Bot is not a participant in the chat ||
|#

{% include [System Errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [API imbot.v2 Change Log](../../change-log.md)
- [{#T}](./chat-add.md)
- [{#T}](./chat-update.md)
- [{#T}](./chat-user-list.md)