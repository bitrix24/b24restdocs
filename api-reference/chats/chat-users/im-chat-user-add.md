# Invite Participants to Chat im.chat.user.add

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: chat participant with permission to add users to the chat

The method `im.chat.user.add` adds users to a chat.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CHAT_ID***
[`integer`](../../data-types.md) | Identifier of the chat.

The chat identifier can be obtained using the [im.chat.get](../im-chat-get.md) method ||
|| **USERS***
[`array`](../../data-types.md) | Array of user identifiers to be added to the chat.

User identifiers can be obtained using the [user.get](../../user/user-get.md) and [user.search](../../user/user-search.md) methods ||
|| **HIDE_HISTORY**
[`string`](../../data-types.md) | Hide chat history for added users:
- `Y` — hide
- `N` — do not hide 

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
    -d '{"CHAT_ID":2935,"USERS":[99,1271],"HIDE_HISTORY":"N"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.chat.user.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CHAT_ID":2935,"USERS":[99,1271],"HIDE_HISTORY":"N","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/im.chat.user.add
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'im.chat.user.add',
            {
                CHAT_ID: 2935,
                USERS: [99, 1271],
                HIDE_HISTORY: 'N'
            }
        );
        
        const result = response.getData().result;
        console.log('Added users to chat:', result);
        
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
                'im.chat.user.add',
                [
                    'CHAT_ID' => 2935,
                    'USERS' => [99, 1271],
                    'HIDE_HISTORY' => 'N'
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding users to chat: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'im.chat.user.add',
        {
            CHAT_ID: 2935,
            USERS: [99, 1271],
            HIDE_HISTORY: 'N'
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
        'im.chat.user.add',
        [
            'CHAT_ID' => 2935,
            'USERS' => [99, 1271],
            'HIDE_HISTORY' => 'N'
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
        "start": 1772452546,
        "finish": 1772452546.179628,
        "duration": 0.1796278953552246,
        "processing": 0,
        "date_start": "2026-03-02T10:55:46+01:00",
        "date_finish": "2026-03-02T10:55:46+01:00",
        "operating_reset_at": 1772453146,
        "operating": 0.1326589584350586
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | `true` if users were added to the chat ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**, **403**

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
|| `CHAT_ID_EMPTY` | Chat ID can't be empty | `CHAT_ID` not provided ||
|| `ACCESS_ERROR` | Action unavailable | Operation not available for this chat ||
|| `ACCESS_ERROR` | It is forbidden to add users to this chat | Users cannot be added to this chat ||
|| `ACCESS_DENIED_EXTEND` | Insufficient permissions to add participants to the chat | Not enough permissions to add participants to the chat ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](../im-chat-add.md)
- [{#T}](../chat-update/im-chat-update-title.md)
- [{#T}](../chat-update/im-chat-update-avatar.md)
- [{#T}](../chat-update/im-chat-update-color.md)
- [{#T}](../im-chat-get.md)
- [{#T}](../im-dialog-get.md)
- [{#T}](./im-chat-user-list.md)
- [{#T}](./im-dialog-users-list.md)
- [{#T}](./im-chat-user-delete.md)
- [{#T}](./im-chat-leave.md)