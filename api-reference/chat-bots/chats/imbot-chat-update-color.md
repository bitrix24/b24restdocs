# Change Chat Color imbot.chat.updateColor

> Scope: [`imbot`](../../scopes/permissions.md)
>
> Who can execute the method: an authorized user of the application that registered the chat bot

The method `imbot.chat.updateColor` updates the chat color for the mobile application.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CHAT_ID***
[`integer`](../../data-types.md) | Identifier of the chat.

The identifier can be obtained using the [imbot.chat.get](./imbot-chat-get.md) method. ||
|| **COLOR***
[`string`](../../data-types.md) | Chat color for the mobile application. Possible values:
- `RED` — red
- `GREEN` — green
- `MINT` — mint
- `LIGHT_BLUE` — light blue
- `DARK_BLUE` — dark blue
- `PURPLE` — purple
- `AQUA` — aqua
- `PINK` — pink
- `LIME` — lime
- `BROWN` — brown
- `AZURE` — azure
- `KHAKI` — khaki
- `SAND` — sand
- `MARENGO` — marengo
- `GRAY` — gray
- `GRAPHITE` — graphite ||
|| **BOT_ID**
[`integer`](../../data-types.md) | Identifier of the chat bot. You can obtain the bot identifier using the [imbot.bot.list](../imbot-bot-list.md) method.

If the parameter is not provided, the method searches for the first bot registered by the current application. ||
|| **CLIENT_ID**
[`string`](../../data-types.md) | This parameter is required only for webhooks. Pass the same CLIENT_ID that was specified during the registration of the chat bot. ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CHAT_ID":2725,"COLOR":"PINK","CLIENT_ID":"**put_your_client_id_here**"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imbot.chat.updateColor
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CHAT_ID":2725,"COLOR":"PINK","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/imbot.chat.updateColor
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'imbot.chat.updateColor',
            {
                CHAT_ID: 2725,
                COLOR: 'PINK'
            }
        );
        
        const result = response.getData().result;
        console.log('Updated chat color:', result);
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
                'imbot.chat.updateColor',
                [
                    'CHAT_ID' => 2725,
                    'COLOR' => 'PINK'
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating chat color: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'imbot.chat.updateColor',
        {
            CHAT_ID: 2725,
            COLOR: 'PINK',
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
        'imbot.chat.updateColor',
        [
            'CHAT_ID' => 2725,
            'COLOR' => 'PINK'
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
        "start": 1771935255,
        "finish": 1771935255.192065,
        "duration": 0.19206500053405762,
        "processing": 0,
        "date_start": "2026-02-24T15:14:15+01:00",
        "date_finish": "2026-02-24T15:14:15+01:00",
        "operating_reset_at": 1771935855,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | `true` if the chat color was updated. ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time. ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "WRONG_COLOR",
    "error_description": "This color currently unavailable"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `CHAT_ID_EMPTY` | Chat ID can't be empty | `CHAT_ID` not provided. ||
|| `ACCESS_ERROR` | Action unavailable | Operation not available for this chat. ||
|| `WRONG_COLOR` | This color currently unavailable | An invalid color was provided. ||
|| `WRONG_REQUEST` | This color currently set or chat doesn't exist | The color is already set or the chat does not exist. ||
|| `BOT_ID_ERROR` | Bot not found | Chat bot not found. ||
|| `APP_ID_ERROR` | Bot was installed by another REST application | Chat bot installed by another application. ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./imbot-chat-add.md)
- [{#T}](./imbot-chat-user-add.md)
- [{#T}](./imbot-chat-set-manager.md)
- [{#T}](./imbot-chat-update-title.md)
- [{#T}](./imbot-chat-update-avatar.md)
- [{#T}](./imbot-chat-get.md)
- [{#T}](./imbot-dialog-get.md)
- [{#T}](./imbot-chat-user-list.md)
- [{#T}](./imbot-chat-user-delete.md)
- [{#T}](./imbot-chat-leave.md)