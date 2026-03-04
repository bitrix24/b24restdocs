# Set or Remove the "Read" Flag for the Chat im.recent.unread

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.recent.unread` sets or removes the "read" flag for the chat.

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
  
The chat identifier can be obtained using the [im.chat.get](../im-chat-get.md) method. The user identifier can be retrieved using the [user.get](../../user/user-get.md) and [user.search](../../user/user-search.md) methods ||
|| **ACTION**
[`string`](../../data-types.md) | Action for the "read" flag:
- `Y` — set the flag
- `N` — remove the flag and mark the dialog as read

Default: `Y` ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"DIALOG_ID":"chat2941","ACTION":"Y"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.recent.unread
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"DIALOG_ID":"chat2941","ACTION":"Y","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/im.recent.unread
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'im.recent.unread',
            {
                DIALOG_ID: 'chat2941',
                ACTION: 'Y'
            }
        );
        
        const result = response.getData().result;
        console.log('Marked dialog as unread:', result);
        
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
                'im.recent.unread',
                [
                    'DIALOG_ID' => 'chat2941',
                    'ACTION' => 'Y'
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error marking dialog as unread: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'im.recent.unread',
        {
            DIALOG_ID: 'chat2941',
            ACTION: 'Y'
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
        'im.recent.unread',
        [
            'DIALOG_ID' => 'chat2941',
            'ACTION' => 'Y'
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
        "start": 1772518597,
        "finish": 1772518597.385986,
        "duration": 0.3859860897064209,
        "processing": 0,
        "date_start": "2026-03-03T09:16:37+01:00",
        "date_finish": "2026-03-03T09:16:37+01:00",
        "operating_reset_at": 1772519197,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | `true` if the flag was successfully set or removed.

Returns `false` if the chat with the specified `DIALOG_ID` was not found ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
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
|| **Code** | **Description** | **Value** ||
|| `DIALOG_ID_EMPTY` | Dialog ID can't be empty | Not provided or provided in an incorrect format `DIALOG_ID` ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./im-recent-pin.md)
- [{#T}](./im-dialog-read-all.md)
- [{#T}](./im-chat-mute.md)
- [{#T}](./im-recent-hide.md)