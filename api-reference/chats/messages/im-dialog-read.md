# Set the "read" flag for messages im.dialog.read

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.dialog.read` sets the "read" flag for dialog messages up to and including the specified message.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **DIALOG_ID***
[`string`](../../data-types.md) | Identifier of the chat in the format:

- `chatXXX` — chat
- `sgXXX` — group or project chat
- `XXX` — identifier of the personal chat user

The chat identifier can be obtained using the [im.chat.get](../im-chat-get.md) method. The user identifier can be obtained using the [user.get](../../user/user-get.md) and [user.search](../../user/user-search.md) methods ||
|| **MESSAGE_ID**
[`integer`](../../data-types.md) | Identifier of the last read message. If not provided, the method sets the read flag for all unread messages ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"DIALOG_ID":"chat1489","MESSAGE_ID":84875}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.dialog.read
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"DIALOG_ID":"chat1489","MESSAGE_ID":84875,"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/im.dialog.read
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'im.dialog.read',
            {
                DIALOG_ID: 'chat1489',
                MESSAGE_ID: 84875
            }
        );

        console.log(response.getData().result);
    }
    catch (error)
    {
        console.error(error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service->core->call(
            'im.dialog.read',
            [
                'DIALOG_ID' => 'chat1489',
                'MESSAGE_ID' => 84875,
            ]
        );

        $result = $response->getResponseData()->getResult();
        print_r($result);
    } catch (Throwable $e) {
        error_log($e->getMessage());
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'im.dialog.read',
        {
            DIALOG_ID: 'chat1489',
            MESSAGE_ID: 84875
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
        'im.dialog.read',
        [
            'DIALOG_ID' => 'chat1489',
            'MESSAGE_ID' => 84875,
        ]
    );

    print_r($result);
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "dialogId": "chat1489",
        "chatId": 1489,
        "lastId": 84875,
        "counter": 3
    },
    "time": {
        "start": 1772624912,
        "finish": 1772624912.615753,
        "duration": 0.6157529354095459,
        "processing": 0,
        "date_start": "2026-03-04T14:48:32+01:00",
        "date_finish": "2026-03-04T14:48:32+01:00",
        "operating_reset_at": 1772625512,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response [(detailed description)](#result).

Returns `false` if the user does not have access to the chat ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Result Object {#result}

#|
|| **Name**
`type` | **Description** ||
|| **dialogId**
[`string`](../../data-types.md) | Identifier of the dialog ||
|| **chatId**
[`integer`](../../data-types.md) | Identifier of the chat ||
|| **lastId**
[`integer`](../../data-types.md) | Identifier of the last read message ||
|| **counter**
[`integer`](../../data-types.md) | Number of unread messages after executing the method ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "DIALOG_ID_EMPTY",
    "error_description": "Dialog ID can't be empty"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `DIALOG_ID_EMPTY` | Dialog ID can't be empty | The `DIALOG_ID` parameter was not provided, was empty, or was in an incorrect format ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./im-dialog-messages-get.md)
- [{#T}](./im-dialog-messages-search.md)
- [{#T}](./im-dialog-unread.md)
- [{#T}](./im-dialog-writing.md)