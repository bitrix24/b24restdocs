# Change Chat Color im.chat.updateColor

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: chat participant with permission to change the appearance

The method `im.chat.updateColor` updates the chat color for the mobile application.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CHAT_ID***
[`integer`](../../data-types.md) | Identifier of the chat.

The chat identifier can be obtained using the [im.chat.get](../im-chat-get.md) method ||
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
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CHAT_ID":2935,"COLOR":"SAND"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.chat.updateColor
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CHAT_ID":2935,"COLOR":"SAND","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/im.chat.updateColor
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'im.chat.updateColor',
            {
                CHAT_ID: 2935,
                COLOR: 'SAND'
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
                'im.chat.updateColor',
                [
                    'CHAT_ID' => 2935,
                    'COLOR' => 'SAND'
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
        'im.chat.updateColor',
        {
            CHAT_ID: 2935,
            COLOR: 'SAND',
        },
        function(result)
        {
            if (result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'im.chat.updateColor',
        [
            'CHAT_ID' => 2935,
            'COLOR' => 'SAND'
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
        "start": 1772459571,
        "finish": 1772459571.876142,
        "duration": 0.8761420249938965,
        "processing": 0,
        "date_start": "2026-03-02T13:52:51+01:00",
        "date_finish": "2026-03-02T13:52:51+01:00",
        "operating_reset_at": 1772460171,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | `true` if the chat color has been updated ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
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
|| `CHAT_ID_EMPTY` | Chat ID can't be empty | `CHAT_ID` not provided ||
|| `ACCESS_ERROR` | Action unavailable | Operation not available for this chat ||
|| `WRONG_COLOR` | This color currently unavailable | Invalid color provided ||
|| `WRONG_REQUEST` | This color currently set or chat doesn't exist | Color already set or chat does not exist ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](../im-chat-add.md)
- [{#T}](../chat-users/im-chat-user-add.md)
- [{#T}](./im-chat-update-title.md)
- [{#T}](./im-chat-update-avatar.md)
- [{#T}](../im-chat-get.md)
- [{#T}](../im-dialog-get.md)
- [{#T}](../chat-users/im-chat-user-list.md)
- [{#T}](../chat-users/im-chat-user-delete.md)
- [{#T}](../chat-users/im-chat-leave.md)