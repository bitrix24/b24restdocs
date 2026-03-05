# Exclude Participants from Chat imbot.chat.user.delete

> Scope: [`imbot`](../../scopes/permissions.md)
>
> Who can execute the method: an authorized user of the application that registered the chat bot.

The method `imbot.chat.user.delete` removes a user from the chat.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CHAT_ID***
[`integer`](../../data-types.md) | The identifier of the chat.

The identifier can be obtained using the [imbot.chat.get](./imbot-chat-get.md) method. ||
|| **USER_ID***
[`integer`](../../data-types.md) | The identifier of the user to be removed from the chat.

You can get the user identifier using the [imbot.chat.user.list](./imbot-chat-user-list.md) method. ||
|| **BOT_ID**
[`integer`](../../data-types.md) | The identifier of the chat bot. You can obtain the bot identifier using the [imbot.bot.list](../imbot-bot-list.md) method.

If this parameter is not provided, the method searches for the first bot registered by the current application. ||
|| **CLIENT_ID**
[`string`](../../data-types.md) | This parameter is required only for webhooks. Pass the same CLIENT_ID that was specified when registering the chat bot. ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CHAT_ID":2725,"USER_ID":1269,"CLIENT_ID":"**put_your_client_id_here**"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imbot.chat.user.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CHAT_ID":2725,"USER_ID":1269,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/imbot.chat.user.delete
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'imbot.chat.user.delete',
            {
                CHAT_ID: 2725,
                USER_ID: 1269,
            }
        );
        
        const result = response.getData().result;
        console.log('User deleted from chat:', result);
        
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
                'imbot.chat.user.delete',
                [
                    'CHAT_ID' => 2725,
                    'USER_ID' => 1269
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error deleting chat user: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'imbot.chat.user.delete',
        {
            CHAT_ID: 2725,
            USER_ID: 1269,
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
        'imbot.chat.user.delete',
        [
            'CHAT_ID' => 2725,
            'USER_ID' => 1269
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
        "start": 1771936672,
        "finish": 1771936672.409352,
        "duration": 0.40935206413269043,
        "processing": 0,
        "date_start": "2026-02-24T15:37:52+01:00",
        "date_finish": "2026-02-24T15:37:52+01:00",
        "operating_reset_at": 1771937272,
        "operating": 0.23420310020446777
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | `true` if the user has been removed from the chat. ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request. ||
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
|| `CHAT_ID_EMPTY` | Chat ID can't be empty | `CHAT_ID` not provided. ||
|| `USER_ID_EMPTY` | User ID can't be empty | `USER_ID` not provided. ||
|| `ACCESS_ERROR` | Action unavailable | Operation not available for this chat. ||
|| `ACCESS_ERROR` | It is forbidden to delete users of this chat | Users cannot be removed from this chat. ||
|| `ACCESS_ERROR` | LEAVE_OWNER_FORBIDDEN | The owner of the chat cannot be removed without transferring rights. ||
|| `WRONG_REQUEST` | You don't have access or user isn't a member of the chat | No permission to delete or user is not a member of the chat. ||
|| `BOT_ID_ERROR` | Bot not found | Chat bot not found. ||
|| `APP_ID_ERROR` | Bot was installed by another REST application | Chat bot was installed by another application. ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./imbot-chat-add.md)
- [{#T}](./imbot-chat-user-add.md)
- [{#T}](./imbot-chat-set-manager.md)
- [{#T}](./imbot-chat-update-title.md)
- [{#T}](./imbot-chat-update-avatar.md)
- [{#T}](./imbot-chat-update-color.md)
- [{#T}](./imbot-chat-get.md)
- [{#T}](./imbot-dialog-get.md)
- [{#T}](./imbot-chat-user-list.md)
- [{#T}](./imbot-chat-leave.md)