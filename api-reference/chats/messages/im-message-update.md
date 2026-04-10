# Update Message im.message.update

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: message author

The method `im.message.update` modifies the text and parameters of an already sent message.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **MESSAGE_ID***
[`integer`](../../data-types.md) | Identifier of the message.

The identifier is returned by the method [im.message.add](./im-message-add.md) ||
|| **MESSAGE**
[`string`](../../data-types.md) | New text of the message. If an empty value is provided, the message will be deleted.

If the parameter is not provided, the message text will remain unchanged ||
|| **ATTACH**
[`object`](../../data-types.md)
[`string`](../../data-types.md) | Attachment with content blocks: images, links, files. To remove the attachment, pass `N` or an empty value.

Read more in the [Attachments](./attachments.md) section ||
|| **KEYBOARD**
[`object`](../../data-types.md)
[`string`](../../data-types.md) | Buttons below the message that the user can interact with. To disable button display, pass `N` or an empty value.

Read more in the [Working with Keyboards](./keyboards.md) article ||
|| **MENU**
[`object`](../../data-types.md)
[`string`](../../data-types.md) | Additional items in the chat's context menu. To disable the display of additional menu items, pass `N` or an empty value.

Read more in the [Context Menu](./menu.md) article ||
|| **URL_PREVIEW**
[`string`](../../data-types.md) | Conversion of links into rich links.

Allowed values:
- `Y` — enabled
- `N` — disabled ||
|| **IS_EDITED**
[`string`](../../data-types.md) | Flag for marking "edited":
- `Y` — mark
- `N` — do not mark

Default is `Y` 

{% note info "" %}

The parameter is applied when updating `ATTACH`. When changing `MESSAGE`, the "edited" label may appear even if `IS_EDITED = 'N'` is provided.

{% endnote %} ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"MESSAGE_ID":34239,"MESSAGE":"Updated text","KEYBOARD":"N"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.message.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"MESSAGE_ID":34239,"MESSAGE":"Updated text","KEYBOARD":"N","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/im.message.update
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'im.message.update',
            {
                MESSAGE_ID: 34239,
                MESSAGE: 'Updated text',
                KEYBOARD: 'N'
            }
        );
        
        const result = response.getData().result;
        console.log(result);
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
                'im.message.update',
                [
                    'MESSAGE_ID' => 34239,
                    'MESSAGE' => 'Updated text',
                    'KEYBOARD' => 'N'
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating message: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'im.message.update',
        {
            MESSAGE_ID: 34239,
            MESSAGE: 'Updated text',
            KEYBOARD: 'N'
        },
        function(result)
        {
            if (result.error())
                console.error(result.error());
            else
                console.log(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'im.message.update',
        [
            'MESSAGE_ID' => 34239,
            'MESSAGE' => 'Updated text',
            'KEYBOARD' => 'N'
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
        "start": 1772623160,
        "finish": 1772623160.616113,
        "duration": 0.6161129474639893,
        "processing": 0,
        "date_start": "2026-03-04T14:19:20+01:00",
        "date_finish": "2026-03-04T14:19:20+01:00",
        "operating_reset_at": 1772623760,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | `true` if the message was updated ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "MESSAGE_ID_ERROR",
    "error_description": "Message ID can't be empty"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `MESSAGE_ID_ERROR` | Message ID can't be empty | `MESSAGE_ID` is missing or invalid ||
|| `CANT_EDIT_MESSAGE` | Time has expired for modification or you don't have access | No permission to edit the message or the modification period has expired ||
|| `ATTACH_ERROR` | Incorrect attach params | Invalid `ATTACH` object ||
|| `ATTACH_OVERSIZE` | You have exceeded the maximum allowable size of attach | The size of `ATTACH` exceeds the allowable limit ||
|| `KEYBOARD_ERROR` | Incorrect keyboard params | Invalid `KEYBOARD` object ||
|| `KEYBOARD_OVERSIZE` | You have exceeded the maximum allowable size of keyboard | The size of `KEYBOARD` exceeds the allowable limit ||
|| `menu_ERROR` | Incorrect menu params | Invalid `MENU` object ||
|| `MENU_OVERSIZE` | You have exceeded the maximum allowable size of menu | The size of `MENU` exceeds the allowable limit ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./im-message-add.md)
- [{#T}](./im-message-delete.md)
- [{#T}](./keyboards.md)
- [{#T}](./attachments.md)
- [{#T}](./menu.md)