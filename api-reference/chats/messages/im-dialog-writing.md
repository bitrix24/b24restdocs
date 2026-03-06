# Send "User is typing" indicator im.dialog.writing

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: chat participant

The method `im.dialog.writing` sends a "User is typing" indicator to the chat.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **DIALOG_ID***
[`string`](../../data-types.md) | Identifier of the chat in the format:

- `chatXXX` — chat
- `sgXXX` — group or project chat
- `XXX` — user identifier for personal chat 

The chat identifier can be obtained using the [im.chat.get](../im-chat-get.md) method. The user identifier can be retrieved using the [user.get](../../user/user-get.md) and [user.search](../../user/user-search.md) methods ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"DIALOG_ID":"chat2935"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.dialog.writing
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"DIALOG_ID":"chat2935","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/im.dialog.writing
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'im.dialog.writing',
            {
                DIALOG_ID: 'chat2935'
            }
        );
        
        const result = response.getData().result;
        console.log('Writing status indicated:', result);
        
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
                'im.dialog.writing',
                [
                    'DIALOG_ID' => 'chat2935'
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error indicating writing status: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'im.dialog.writing',
        {
            DIALOG_ID: 'chat2935'
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
        'im.dialog.writing',
        [
            'DIALOG_ID' => 'chat2935'
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
        "start": 1772548455,
        "finish": 1772548455.702725,
        "duration": 0.7027249336242676,
        "processing": 0,
        "date_start": "2026-03-03T14:34:15+01:00",
        "date_finish": "2026-03-03T14:34:15+01:00",
        "operating_reset_at": 1772549055,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | `true` if the command to send the indicator was accepted ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "DIALOG_ID_EMPTY",
    "error_description": "Dialog ID can't be empty"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `DIALOG_ID_EMPTY` | Dialog ID can't be empty | Invalid `DIALOG_ID` ||
|| `ACCESS_ERROR` | Action unavailable | No access to the specified chat ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./im-message-add.md)
- [{#T}](./im-dialog-read.md)
- [{#T}](./im-dialog-unread.md)