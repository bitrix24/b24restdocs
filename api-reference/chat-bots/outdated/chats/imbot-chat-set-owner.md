# Change Chat Owner via imbot.chat.setOwner

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`imbot`](../../../scopes/permissions.md)
>
> Who can execute the method: an authorized user of the application that registered the chat bot.

{% note warning "DEPRECATED" %}

The development of this method has been halted. Please use [imbot.v2.Chat.setOwner](../../chat-bots-v2/imbot.v2/chats/chat-set-owner.md).

{% endnote %}

The method `imbot.chat.setOwner` changes the owner of the chat.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **CHAT_ID*** 
[`integer`](../../../data-types.md) | The identifier of the chat.

The identifier can be obtained using the [imbot.chat.get](./imbot-chat-get.md) method. ||
|| **USER_ID*** 
[`integer`](../../../data-types.md) | The identifier of the new chat owner.

You can get the user identifier using the [user.get](../../../user/user-get.md) and [user.search](../../../user/user-search.md) methods. ||
|| **BOT_ID** 
[`integer`](../../../data-types.md) | The identifier of the chat bot. You can obtain the bot identifier using the [imbot.bot.list](../bots/imbot-bot-list.md) method.

If this parameter is not provided, the method searches for the first bot registered by the current application. ||
|| **CLIENT_ID** 
[`string`](../../../data-types.md) | A technical parameter for scenarios without `clientId` in authorization.

If provided, it is used as `custom{CLIENT_ID}` to identify the application. ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CHAT_ID":2727,"USER_ID":1269}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imbot.chat.setOwner
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CHAT_ID":2727,"USER_ID":1269,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/imbot.chat.setOwner
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'imbot.chat.setOwner',
            {
                CHAT_ID: 2727,
                USER_ID: 1269,
            }
        );
        
        const result = response.getData().result;
        console.log('Chat owner set:', result);
        
        processResult(result);
    }
    catch( error )
    {
        console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'imbot.chat.setOwner',
                [
                    'CHAT_ID' => 2727,
                    'USER_ID' => 1269
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error setting chat owner: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'imbot.chat.setOwner',
        {
            CHAT_ID: 2727,
            USER_ID: 1269,
        },
        function (result)
        {
            if (result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'imbot.chat.setOwner',
        [
            'CHAT_ID' => 2727,
            'USER_ID' => 1269
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1771952858,
        "finish": 1771952858.856461,
        "duration": 0.8564610481262207,
        "processing": 0,
        "date_start": "2026-02-24T17:07:38+01:00",
        "date_finish": "2026-02-24T17:07:38+01:00",
        "operating_reset_at": 1771953458,
        "operating": 0
    }
}
```

### Returned Data

#| 
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | `true` if the chat owner has been changed. ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request. ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "CHAT_ID_EMPTY",
    "error_description": "Chat ID can't be empty"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#| 
|| **Code** | **Description** | **Value** ||
|| `CHAT_ID_EMPTY` | Chat ID can't be empty | `CHAT_ID` not provided. ||
|| `USER_ID_EMPTY` | User ID can't be empty | `USER_ID` not provided. ||
|| `ACCESS_ERROR` | Action unavailable | Cannot change the owner of a group chat. ||
|| `WRONG_REQUEST` | Change owner can only owner and user must be member in chat | Only the current owner can change the owner, and the user must be a participant in the chat. ||
|| `BOT_ID_ERROR` | Bot not found | Chat bot not found. ||
|| `APP_ID_ERROR` | Bot was installed by another rest application | Chat bot installed by another application. ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./imbot-chat-add.md)
- [{#T}](./imbot-chat-user-add.md)
- [{#T}](./imbot-chat-update-title.md)
- [{#T}](./imbot-chat-update-avatar.md)
- [{#T}](./imbot-chat-update-color.md)
- [{#T}](./imbot-chat-get.md)
- [{#T}](./imbot-dialog-get.md)
- [{#T}](./imbot-chat-user-list.md)
- [{#T}](./imbot-chat-user-delete.md)
- [{#T}](./imbot-chat-leave.md)