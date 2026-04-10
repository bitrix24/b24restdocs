# Create an Object Based on the Message im.message.share

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: chat participant

The method `im.message.share` creates an object based on a message.

## Method Parameters

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **MESSAGE_ID***
[`integer`](../../data-types.md) | Identifier of the message.

The identifier can be obtained using the method [im.dialog.messages.get](./im-dialog-messages-get.md) ||
|| **DIALOG_ID***
[`string`](../../data-types.md) | Identifier of the chat in the format:

- `chatXXX` — chat
- `sgXXX` — group or project chat
- `XXX` — identifier of the user in a personal chat 

The chat identifier can be obtained using the method [im.chat.get](../im-chat-get.md). The user identifier can be obtained using the methods [user.get](../../user/user-get.md) and [user.search](../../user/user-search.md) ||
|| **TYPE***
[`string`](../../data-types.md) | Type of the object being created.

Allowed values:
- `CHAT` — chat
- `TASK` — task
- `POST` — post in the feed
- `CALEND` — calendar event ||
|#

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"MESSAGE_ID":34261,"DIALOG_ID":"chat2941","TYPE":"TASK"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.message.share
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"MESSAGE_ID":34261,"DIALOG_ID":"chat2941","TYPE":"TASK","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/im.message.share
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'im.message.share',
            {
                MESSAGE_ID: 34261,
                DIALOG_ID: 'chat2941',
                TYPE: 'TASK'
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
                'im.message.share',
                [
                    'MESSAGE_ID' => 34261,
                    'DIALOG_ID' => 'chat2941',
                    'TYPE' => 'TASK'
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error sharing message: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'im.message.share',
        {
            MESSAGE_ID: 34261,
            DIALOG_ID: 'chat2941',
            TYPE: 'TASK'
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
        'im.message.share',
        [
            'MESSAGE_ID' => 34261,
            'DIALOG_ID' => 'chat2941',
            'TYPE' => 'TASK'
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
        "start": 1772632079,
        "finish": 1772632079.779834,
        "duration": 0.7798340320587158,
        "processing": 0,
        "date_start": "2026-03-04T16:47:59+01:00",
        "date_finish": "2026-03-04T16:47:59+01:00",
        "operating_reset_at": 1772632679,
        "operating": 0.5939757823944092
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | `true` if the object was created ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "PARAMS_ERROR",
    "error_description": "Incorrect params"
}
```

{% include notitle [Error Handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `MESSAGE_ID_ERROR` | Message ID can't be empty | `MESSAGE_ID` is missing or incorrect ||
|| `DIALOG_ID_EMPTY` | Dialog ID can't be empty | `DIALOG_ID` is missing or incorrect ||
|| `ACCESS_ERROR` | You do not have access to the specified dialog | No access to the dialog ||
|| `PARAMS_ERROR` | Incorrect params | Incorrect request parameters ||
|#

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./im-message-add.md)
- [{#T}](./im-message-command.md)
- [{#T}](./im-dialog-messages-get.md)