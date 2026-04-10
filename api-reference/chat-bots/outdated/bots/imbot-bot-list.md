# Get the List of Chat Bots imbot.bot.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`imbot`](../../../scopes/permissions.md)
>
> Who can execute the method: authorized application user

{% note warning "DEPRECATED" %}

Development of this method has been halted. Please use [imbot.v2.Bot.list](../../chat-bots-v2/imbot.v2/bots/bot-list.md).

{% endnote %}

The method `imbot.bot.list` returns a list of registered chat bots.

No parameters required.

## Code Examples

{% include [Examples Note](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST /
      -H "Content-Type: application/json" /
      -H "Accept: application/json" /
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imbot.bot.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST /
      -H "Content-Type: application/json" /
      -H "Accept: application/json" /
      -d '{"auth":"**put_access_token_here**"}' /
      https://**put_your_bitrix24_address**/rest/imbot.bot.list
    ```

- JS

    ```js
    try {
      const response = await $b24.callMethod('imbot.bot.list', {});
      const { result } = response.getData();
      console.log('Bots:', result);
    } catch (error) {
      console.error('Error getting bot list:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call('imbot.bot.list', []);

        $result = $response
            ->getResponseData()
            ->getResult();

        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            print_r($result->data());
        }
    } catch (Throwable $exception) {
        error_log($exception->getMessage());
        echo 'Error getting bot list: ' . $exception->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'imbot.bot.list',
        {},
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

    $result = CRest::call('imbot.bot.list', []);

    if (!empty($result['error'])) {
        echo 'Error: ' . $result['error_description'];
    } else {
        print_r($result['result']);
    }
    ```

{% endlist %}

## Response Handling

HTTP Code: **200**

```json
{
    "result": {
        "39": {
            "ID": 39,
            "NAME": "NewBot",
            "CODE": "newbot",
            "OPENLINE": "N"
        }
    },
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
[`object`](../../../data-types.md) | An object where the top-level key is `BOT_ID`, and the value contains the bot data. The structure of the element is described in detail [below](#bot-item) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

### Element /{BOT_ID/} {#bot-item}

#|
|| **Name**
`Type` | **Description** ||
|| **ID**
[`integer`](../../../data-types.md) | Identifier of the chat bot ||
|| **NAME**
[`string`](../../../data-types.md) | Full name of the chat bot ||
|| **CODE**
[`string`](../../../data-types.md) | Code of the chat bot ||
|| **OPENLINE**
[`string`](../../../data-types.md) | Indicator of open line support: `Y` or `N` ||
|#

## Error Handling

HTTP Status: **403**

```json
{
    "error": "WRONG_AUTH_TYPE",
    "error_description": "Access for this method not allowed by session authorization."
}
```

{% include notitle [Error Handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `WRONG_AUTH_TYPE` | Access for this method not allowed by session authorization. | The method was called with session authorization instead of OAuth or webhook ||
|#

{% include [System Errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [Create a Chat-Bot imbot.register](./imbot-register.md)
- [Update Chat-Bot imbot.update](./imbot-update.md)
- [Unregister Chat Bot imbot.unregister](./imbot-unregister.md)
