# Exclude Participants from Chat im.chat.user.delete

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: chat participant with permission to exclude participants from the chat

The method `im.chat.user.delete` removes a user from the chat.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CHAT_ID***
[`integer`](../../data-types.md) | Identifier of the chat.

The chat identifier can be obtained using the [im.chat.get](../im-chat-get.md) method ||
|| **USER_ID***
[`integer`](../../data-types.md) | Identifier of the user to be excluded from the chat.

The identifier can be obtained using the [im.chat.user.list](../chat-users/im-chat-user-list.md) method ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CHAT_ID":2935,"USER_ID":1291}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.chat.user.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CHAT_ID":2935,"USER_ID":1291,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/im.chat.user.delete
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'im.chat.user.delete',
            {
                CHAT_ID: 2935,
                USER_ID: 1291
            }
        );
        
        const result = response.getData().result;
        console.log('Deleted user from chat:', result);
        
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
                'im.chat.user.delete',
                [
                    'CHAT_ID' => 2935,
                    'USER_ID' => 1291
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error deleting user from chat: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'im.chat.user.delete',
        {
            CHAT_ID: 2935,
            USER_ID: 1291,
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
        'im.chat.user.delete',
        [
            'CHAT_ID' => 2935,
            'USER_ID' => 1291
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
        "start": 1772482407,
        "finish": 1772482407.879069,
        "duration": 0.8790690898895264,
        "processing": 0,
        "date_start": "2026-03-02T13:13:27+01:00",
        "date_finish": "2026-03-02T13:13:27+01:00",
        "operating_reset_at": 1772483007,
        "operating": 0.7018318176269531
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | `true` if the user has been removed from the chat ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "USER_ID_EMPTY",
    "error_description": "User ID can't be empty"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `CHAT_ID_EMPTY` | Chat ID can't be empty | `CHAT_ID` not provided ||
|| `USER_ID_EMPTY` | User ID can't be empty | `USER_ID` not provided ||
|| `ACCESS_ERROR` | Action unavailable | Operation not available for this chat ||
|| `ACCESS_ERROR` | It is forbidden to delete users of this chat | Users cannot be removed from this chat ||
|| `ACCESS_DENIED_KICK` | Insufficient rights to exclude participants from the chat | Insufficient rights to remove participants from the chat ||
|| `USER_NOT_FOUND` | The specified user is not in the chat | User not found among the participants of the specified chat ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](../im-chat-add.md)
- [{#T}](./im-chat-user-add.md)
- [{#T}](../chat-update/im-chat-update-title.md)
- [{#T}](../chat-update/im-chat-update-avatar.md)
- [{#T}](../chat-update/im-chat-update-color.md)
- [{#T}](../im-chat-get.md)
- [{#T}](../im-dialog-get.md)
- [{#T}](./im-chat-user-list.md)
- [{#T}](./im-dialog-users-list.md)
- [{#T}](./im-chat-leave.md)