# Assign Chat Owner imbot.v2.Chat.setOwner

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`imbot`](../../../../scopes/permissions.md)
>
> Who can execute the method: the owner of the registered bot

The method `imbot.v2.Chat.setOwner` transfers chat ownership to another user.

By default, the bot must be the chat owner. If the permission to change settings in the chat (`permissions.manageSettings` in the [imbot.v2.Chat.get](./chat-get.md) response) is granted to managers, the method is also available to a bot with the manager role. The method is not available in personal chats and Open Channel chats.

After the transfer, the previous owner remains a chat participant. The method does not remove the manager role from them — to do this, call [imbot.v2.Chat.Manager.delete](./chat-manager-delete.md).

{% note warning "" %}

The method does not check whether the `userId` user exists or is a participant of the chat. If you pass the ID of a user who is not in the chat, the user becomes the owner but is not added to the chat and gets no access to a private chat. Add the user with the [imbot.v2.Chat.User.add](./chat-user-add.md) method first.

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#| 
|| **Name** 
`Type` | **Description** ||
|| **botId*** 
[`integer`](../../../../data-types.md) | Bot ID ||
|| **botToken** 
[`string`](../../../../data-types.md) | Bot token. Required for webhook authorization, not needed for OAuth.

Pass the same `botToken` that you specified when registering the bot ||
|| **dialogId*** 
[`string`](../../../../data-types.md) | ID of the group chat in the [dialogId format](../../index.md#dialog-id): `chat{chatId}` ||
|| **userId*** 
[`integer`](../../../../data-types.md) | ID of the new chat owner. The user must be a participant of the chat — see the warning above ||
|#

## Code Examples

{% include [Note on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"botId":456,"botToken":"my_bot_token","dialogId":"chat5","userId":1}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imbot.v2.Chat.setOwner
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"botId":456,"dialogId":"chat5","userId":1,"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/imbot.v2.Chat.setOwner
    ```

- JS

    ```js
    try {
      const response = await $b24.callMethod('imbot.v2.Chat.setOwner', {
        botId: 456,
        dialogId: 'chat5',
        userId: 1,
      });

      const { result } = response.getData();
      console.log('result:', result);
    } catch (error) {
      console.error('Error:', error);
    }
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.imbot.v2.chat.set_owner(
            bot_id=456,
            dialog_id="chat5",
            user_id=1,
        ).response
        result = bitrix_response.result
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'imbot.v2.Chat.setOwner',
                [
                    'botId' => 456,
                    'dialogId' => 'chat5',
                    'userId' => 1,
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
        'imbot.v2.Chat.setOwner',
        {
            botId: 456,
            dialogId: 'chat5',
            userId: 1,
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
        'imbot.v2.Chat.setOwner',
        [
            'botId' => 456,
            'dialogId' => 'chat5',
            'userId' => 1,
        ]
    );

    if (!empty($result['error'])) {
        echo 'Error: ' . $result['error_description'];
    } else {
        echo 'Owner changed';
    }
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "imbot.v2.Chat.setOwner", b24.Params{
    	"botId":    456,
    	"botToken": "my_bot_token",
    	"dialogId": "chat5",
    	"userId":   1,
    })
    if err != nil {
    	return fmt.Errorf("imbot.v2.Chat.setOwner: %w", err)
    }

    var item struct {
    	Result bool `json:"result"`
    }
    if err := json.Unmarshal(res.Result, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.Result)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "result": true
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
[`object`](../../../../data-types.md) | Result of the operation ||
|| **result.result** 
[`boolean`](../../../../data-types.md) | `true` if the owner change was successful ||
|| **time** 
[`time`](../../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ACCESS_DENIED",
    "error_description": "ACCESS_DENIED"
}
```

{% include notitle [Error Handling](../../../../../_includes/error-info.md) %}

### Possible Error Codes

#| 
|| **Code** | **Description** | **Value** ||
|| `BOT_TOKEN_NOT_SPECIFIED` | Bot token not specified (botToken is required for webhook auth) | `botToken` is not specified. Required for webhook authorization ||
|| `BOT_ID_REQUIRED` | botId is required | `botId` is not specified ||
|| `BOT_NOT_FOUND` | Bot not found | Bot not found ||
|| `BOT_OWNERSHIP_ERROR` | Bot was installed by another rest application | The bot is registered by another application ||
|| `CHAT_NOT_FOUND` | CHAT_NOT_FOUND | No chat found with the specified `dialogId` ||
|| `ACCESS_DENIED` | ACCESS_DENIED | The bot has no permission to transfer ownership (the owner role is required by default), the bot is not a participant of a private chat, or this is a personal chat or an Open Channel chat ||
|#

{% include [System Errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [API Change Log for imbot.v2](../../change-log.md)
- [{#T}](./chat-manager-add.md)
- [{#T}](./chat-get.md)
- [{#T}](./chat-user-add.md)
- [{#T}](./chat-leave.md)
- [{#T}](./index.md)
- [{#T}](../../migration.md)