# Unregister Chat Bot imbot.unregister

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`imbot`](../../../scopes/permissions.md)
>
> Who can execute the method: an authorized user of the application that registered the chat bot

{% note warning "DEPRECATED" %}

Development of this method has been halted. Please use [imbot.v2.Bot.unregister](../../chat-bots-v2/imbot.v2/bots/bot-unregister.md).

{% endnote %}

The method `imbot.unregister` removes a chat bot.

{% note warning "" %}

When deleting the bot, personal chats with users will also be deleted.

{% endnote %}

## Method Parameters

{% include [Footnote about parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **BOT_ID*** 
[`integer`](../../../data-types.md) | The identifier of the chat bot. The value must be greater than `0`.

You can obtain the bot identifier using the [imbot.bot.list](./imbot-bot-list.md) method. ||
|| **CLIENT_ID**
[`string`](../../../data-types.md) | This parameter is required only for webhooks. Pass the same CLIENT_ID that was specified when registering the chat bot. ||
|#

## Code Examples

{% include [Footnote about examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST /
      -H "Content-Type: application/json" /
      -H "Accept: application/json" /
      -d '{"BOT_ID":39,"CLIENT_ID":"**put_your_client_id_here**"}' /
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imbot.unregister
    ```

- cURL (OAuth)

    ```bash
    curl -X POST /
      -H "Content-Type: application/json" /
      -H "Accept: application/json" /
      -d '{"BOT_ID":39,"auth":"**put_access_token_here**"}' /
      https://**put_your_bitrix24_address**/rest/imbot.unregister
    ```

- JS

    ```js
    try {
      const response = await $b24.callMethod('imbot.unregister', {
        BOT_ID: 39,
      });

      const { result } = response.getData();
      console.log('Unregistered:', result);
    } catch (error) {
      console.error('Error unregistering bot:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'imbot.unregister',
                [
                    'BOT_ID' => 39,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            echo 'Unregistered: ' . ($result->data() ? 'true' : 'false');
        }
    } catch (Throwable $exception) {
        error_log($exception->getMessage());
        echo 'Error unregistering bot: ' . $exception->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'imbot.unregister',
        {
            BOT_ID: 39,
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
        'imbot.unregister',
        [
            'BOT_ID' => 39,
        ]
    );

    if (!empty($result['error'])) {
        echo 'Error: ' . $result['error_description'];
    } else {
        echo 'Unregistered: ' . ($result['result'] ? 'true' : 'false');
    }
    ```

{% endlist %}

## Response Handling

HTTP Code: **200**

```json
{
    "result": true,
    "time": {
        "start": 1728626400.123,
        "finish": 1728626400.234,
        "duration": 0.111,
        "processing": 0.045,
        "date_start": "2024-10-11T10:00:00+02:00",
        "date_finish": "2024-10-11T10:00:00+02:00",
        "operating_reset_at": 1762349466,
        "operating": 0
    }
}
```

## Returned Data

#|
|| **Name**
`Type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | `true` if the chat bot was deleted without error ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "BOT_ID_ERROR",
    "error_description": "Bot not found"
}
```

{% include notitle [Error Handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `WRONG_AUTH_TYPE` | Access for this method not allowed by session authorization. | The method was called with session authorization instead of OAuth or webhook. ||
|| `ACCESS_DENIED` | Access denied! Client ID not specified | Unable to determine the application: `clientId` authorization is missing and `CLIENT_ID` was not provided. ||
|| `BOT_ID_ERROR` | Bot not found | Bot not found. ||
|| `APP_ID_ERROR` | Bot was installed by another REST application | The provided `BOT_ID` belongs to another application. ||
|| `WRONG_REQUEST` | Bot can't be deleted | The bot cannot be deleted. ||
|#

{% include [System Errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [Create a Chat-Bot imbot.register](./imbot-register.md)
- [Update Chat-Bot imbot.update](./imbot-update.md)
- [Get the List of Chat Bots imbot.bot.list](./imbot-bot-list.md)
- [About Events](../events/index.md)
