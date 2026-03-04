# Update Chat Avatar im.chat.updateAvatar

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: chat participant with permission to change the appearance

The method `im.chat.updateAvatar` updates the chat avatar.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CHAT_ID***
[`integer`](../../data-types.md) | Identifier of the chat.

The chat identifier can be obtained using the [im.chat.get](../im-chat-get.md) method ||
|| **AVATAR***
[`string`](../../data-types.md) | Image in [Base64](../../files/how-to-upload-files.md) format.

Maximum image size is 5000x5000 ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CHAT_ID":2935,"AVATAR":"/9j/4QAYRXhpZgAAS...CgCgCgCgP/9k="}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.chat.updateAvatar
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CHAT_ID":2935,"AVATAR":"/9j/4QAYRXhpZgAAS...CgCgCgCgP/9k=","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/im.chat.updateAvatar
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'im.chat.updateAvatar',
            {
                CHAT_ID: 2935,
                AVATAR: '/9j/4QAYRXhpZgAAS...CgCgCgCgP/9k='
            }
        );
        
        const result = response.getData().result;
        console.log('Updated chat avatar:', result);
        
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
                'im.chat.updateAvatar',
                [
                    'CHAT_ID' => 2935,
                    'AVATAR' => '/9j/4QAYRXhpZgAAS...CgCgCgCgP/9k='
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating chat avatar: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'im.chat.updateAvatar',
        {
            CHAT_ID: 2935,
            AVATAR: '/9j/4QAYRXhpZgAAS...CgCgCgCgP/9k=',
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
        'im.chat.updateAvatar',
        [
            'CHAT_ID' => 2935,
            'AVATAR' => '/9j/4QAYRXhpZgAAS...CgCgCgCgP/9k='
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
        "start": 1772459338,
        "finish": 1772459339.565761,
        "duration": 1.5657610893249512,
        "processing": 1,
        "date_start": "2026-03-02T13:48:58+01:00",
        "date_finish": "2026-03-02T13:48:59+01:00",
        "operating_reset_at": 1772459938,
        "operating": 0.7875189781188965
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | `true` if the chat avatar has been updated ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "AVATAR_ERROR",
    "error_description": "Avatar incorrect type"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `CHAT_ID_EMPTY` | Chat ID can't be empty | `CHAT_ID` not provided ||
|| `ACCESS_DENIED` | Access denied | Insufficient permissions to change the avatar ||
|| `ACCESS_ERROR` | Action unavailable | Operation not available for this chat ||
|| `ACCESS_ERROR` | The avatar of this chat cannot be changed | Cannot change the avatar of this chat ||
|| `AVATAR_ERROR` | Avatar incorrect type | The file is not of image type ||
|| `AVATAR_ERROR` | Avatar incorrect size (max 5000x5000) | Image size exceeds the limit ||
|| `WRONG_REQUEST` | Chat doesn't exist | The specified chat does not exist ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](../im-chat-add.md)
- [{#T}](../chat-users/im-chat-user-add.md)
- [{#T}](./im-chat-update-title.md)
- [{#T}](./im-chat-update-color.md)
- [{#T}](../im-chat-get.md)
- [{#T}](../im-dialog-get.md)
- [{#T}](../chat-users/im-chat-user-list.md)
- [{#T}](../chat-users/im-chat-user-delete.md)
- [{#T}](../chat-users/im-chat-leave.md)