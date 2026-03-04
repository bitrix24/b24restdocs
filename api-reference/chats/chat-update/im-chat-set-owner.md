# Change Chat Owner im.chat.setOwner

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: chat owner

The method `im.chat.setOwner` changes the owner of the chat.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CHAT_ID***
[`integer`](../../data-types.md) | Identifier of the chat.

The chat identifier can be obtained using the [im.chat.get](../im-chat-get.md) method. ||
|| **USER_ID***
[`integer`](../../data-types.md) | Identifier of the new chat owner.

The identifier can be obtained using the [im.chat.user.list](../chat-users/im-chat-user-list.md) method. ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CHAT_ID":2935,"USER_ID":1271}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.chat.setOwner
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CHAT_ID":2935,"USER_ID":1271,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/im.chat.setOwner
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'im.chat.setOwner',
            {
                CHAT_ID: 2935,
                USER_ID: 1271
            }
        );
        
        const result = response.getData().result;
        console.log('Set chat owner:', result);
        
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
                'im.chat.setOwner',
                [
                    'CHAT_ID' => 2935,
                    'USER_ID' => 1271
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error setting chat owner: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'im.chat.setOwner',
        {
            CHAT_ID: 2935,
            USER_ID: 1271,
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
        'im.chat.setOwner',
        [
            'CHAT_ID' => 2935,
            'USER_ID' => 1271
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
        "start": 1772480720,
        "finish": 1772480720.22065,
        "duration": 0.22064995765686035,
        "processing": 0,
        "date_start": "2026-03-02T16:45:20+01:00",
        "date_finish": "2026-03-02T16:45:20+01:00",
        "operating_reset_at": 1772481320,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | `true` if the chat owner was successfully changed. ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time. ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "CHAT_ID_EMPTY",
    "error_description": "Chat ID can't be empty"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `CHAT_ID_EMPTY` | Chat ID can't be empty | `CHAT_ID` not provided. ||
|| `USER_ID_EMPTY` | User ID can't be empty | `USER_ID` not provided. ||
|| `ACCESS_ERROR` | Action unavailable | Operation not available for this chat. ||
|| `WRONG_REQUEST` | Change owner can only owner and user must be member in chat | Only the current owner can change the owner, and the new owner must be a participant in the chat. ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](../im-chat-add.md)
- [{#T}](../chat-users/im-chat-user-add.md)
- [{#T}](./im-chat-update-title.md)
- [{#T}](./im-chat-update-avatar.md)
- [{#T}](./im-chat-update-color.md)
- [{#T}](../im-chat-get.md)
- [{#T}](../im-dialog-get.md)
- [{#T}](../chat-users/im-chat-user-list.md)
- [{#T}](../chat-users/im-chat-user-delete.md)
- [{#T}](../chat-users/im-chat-leave.md)