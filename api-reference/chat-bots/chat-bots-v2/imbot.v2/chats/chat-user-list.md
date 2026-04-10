# List of Chat Participants imbot.v2.Chat.User.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`imbot`](../../../../scopes/permissions.md)
>
> Who can execute the method: owner of the registered bot

The method `imbot.v2.Chat.User.list` returns a list of chat participants.

## Method Parameters

{% include [Footnote on parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **botId***
[`integer`](../../../../data-types.md) | Bot ID ||
|| **botToken**
[`string`](../../../../data-types.md) | Unique authorization token for the bot. Required for webhook authorization, not needed for OAuth.

Pass the same botToken that was specified during the chat bot registration ||
|| **dialogId***
[`string`](../../../../data-types.md) | Dialog ID. For group chats — `chat{chatId}` ||
|| **order**
[`object`](../../../../data-types.md) | Sorting.

Available fields: `id`, `lastSendMessageId`, `userId`. Value: `ASC` or `DESC`.

Example: `{"id": "ASC"}` ||
|| **limit**
[`integer`](../../../../data-types.md) | Number of records (1–200). Default is `50` ||
|#

## Code Examples

{% include [Footnote on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"botId":456,"botToken":"my_bot_token","dialogId":"chat5","order":{"id":"ASC"},"limit":50}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imbot.v2.Chat.User.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"botId":456,"dialogId":"chat5","order":{"id":"ASC"},"limit":50,"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/imbot.v2.Chat.User.list
    ```

- JS

    ```js
    try {
      const response = await $b24.callMethod('imbot.v2.Chat.User.list', {
        botId: 456,
        dialogId: 'chat5',
        order: { id: 'ASC' },
        limit: 50,
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
                'imbot.v2.Chat.User.list',
                [
                    'botId' => 456,
                    'dialogId' => 'chat5',
                    'order' => ['id' => 'ASC'],
                    'limit' => 50,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'result: ' . print_r($result, true);
    } catch (Throwable $exception) {
        error_log($exception->getMessage());
        echo 'Error: ' . $exception->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'imbot.v2.Chat.User.list',
        {
            botId: 456,
            dialogId: 'chat5',
            order: { id: 'ASC' },
            limit: 50,
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
        'imbot.v2.Chat.User.list',
        [
            'botId' => 456,
            'dialogId' => 'chat5',
            'order' => ['id' => 'ASC'],
            'limit' => 50,
        ]
    );

    if (!empty($result['error'])) {
        echo 'Error: ' . $result['error_description'];
    } else {
        foreach ($result['result'] as $user) {
            echo $user['id'] . ': ' . $user['name'] . "\n";
        }
    }
    ```

{% endlist %}

## Response Handling

HTTP Code: **200**

```json
{
    "result": [
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
            "extranet": false,
            "bot": false,
            "status": "online",
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
            "extranet": false,
            "bot": false,
            "status": "online",
            "type": "employee"
        }
    ],
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
[`User[]`](../../entities.md#user) | Array of chat participants. Field descriptions for the object — [User](../../entities.md#user) ||
|| **time**
[`time`](../../../../data-types.md#time) | Information about the request execution time ||
|#

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
|| `BOT_TOKEN_NOT_SPECIFIED` | Bot token is not specified | `botToken` is required for webhook authorization ||
|| `BOT_ID_REQUIRED` | Bot ID is required | `botId` is required ||
|| `BOT_NOT_FOUND` | Bot not found | Bot not found ||
|| `BOT_OWNERSHIP_ERROR` | Bot is registered by another application | Bot is registered by another application ||
|| `ACCESS_DENIED` | Access denied | Bot is not a participant in the chat ||
|#

{% include [System Errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [API Change Log for imbot.v2](../../change-log.md)
- [{#T}](./chat-user-add.md)
- [{#T}](./chat-user-delete.md)
- [{#T}](./chat-manager-add.md)