# Get Chat Data with imbot.dialog.get

> Scope: [`imbot`](../../scopes/permissions.md)
>
> Who can execute the method: user with access to the chat

The method `imbot.dialog.get` returns chat data.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **DIALOG_ID***
[`string`](../../data-types.md) | Identifier of the object.

Supported formats:
- `XXX` — user identifier, which can be obtained via [user.get](../../user/user-get.md) or [user.search](../../user/user-search.md). Used to retrieve data about personal chats.
- `chatXXX`, where `XXX` is the identifier of the group chat, which can be obtained via [imbot.chat.get](../chats/imbot-chat-get.md) ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"DIALOG_ID":"chat2725"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imbot.dialog.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"DIALOG_ID":"chat2725","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/imbot.dialog.get
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'imbot.dialog.get',
            {
                DIALOG_ID: 'chat2725',
            }
        );
        
        const result = response.getData().result;
        console.log('Dialog data:', result);
        
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
                'imbot.dialog.get',
                [
                    'DIALOG_ID' => 'chat2725'
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting dialog: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'imbot.dialog.get',
        {
            DIALOG_ID: 'chat2725'
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
        'imbot.dialog.get',
        [
            'DIALOG_ID' => 'chat2725'
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
    "result": {
        "id": 2725,
        "parent_chat_id": 0,
        "parent_message_id": 0,
        "name": "New Chat Name",
        "description": "Important News",
        "owner": 1291,
        "extranet": false,
        "avatar": "https://cdn.com.bitrix24.com/b13743910/resize_cache/33079/ff58db95aecdfa09ae61b51b5fd8f63f/im/708/70810b67c7c206ca3477933063b8ebbc/zuc0242aflwpzacvndxwnfhuxhzvgjaq",
        "color": "#f76187",
        "type": "chat",
        "counter": 0,
        "user_counter": 2,
        "message_count": 11,
        "unread_id": 0,
        "restrictions": {
            "avatar": true,
            "rename": true,
            "extend": true,
            "call": true,
            "mute": true,
            "leave": true,
            "leave_owner": true,
            "send": true,
            "user_list": true
            },
        "last_message_id": 33807,
        "last_id": 33807,
        "marked_id": 0,
        "disk_folder_id": 0,
        "entity_type": "CHAT",
        "entity_id": "13",
        "entity_data_1": "",
        "entity_data_2": "",
        "entity_data_3": "",
        "mute_list": [],
        "date_create": "2026-02-24T13:19:39+01:00",
        "message_type": "C",
        "public": "",
        "role": "member",
        "entity_link": {
            "type": "CHAT",
            "url": "",
            "id": "13"
            },
        "text_field_enabled": true,
        "background_id": null,
        "permissions": {
            "manage_users_add": "member",
            "manage_users_delete": "manager",
            "manage_ui": "member",
            "manage_settings": "owner",
            "manage_messages": "member",
            "can_post": "member"
            },
        "is_new": false,
        "readed_list": [
        {
            "user_id": 1291,
            "user_name": "MyBot",
            "message_id": 33807,
            "date": null
        }
        ],
        "manager_list": [1291],
        "last_message_views": {
            "message_id": 33807,
            "first_viewers": [
                {
                "user_id": 1271,
                "user_name": "Employee",
                "date": "2026-02-24T15:41:17+01:00"
                }
            ],
            "count_of_viewers": 0
            },
        "dialog_id": "chat2725"
    },
    "time": {
        "start": 1771937178,
        "finish": 1771937178.934208,
        "duration": 0.9342079162597656,
        "processing": 0,
        "date_start": "2026-02-24T15:46:18+01:00",
        "date_finish": "2026-02-24T15:46:18+01:00",
        "operating_reset_at": 1771937778,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Object with [chat description](./fields.md) ||
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
|| `DIALOG_ID_EMPTY` | Dialog ID can't be empty | No `DIALOG_ID` provided or it is invalid ||
|| `ACCESS_ERROR` | You do not have access to the specified dialog | No access to the specified dialog ||
|| `ACCESS_ERROR` | You don't have access to this chat | No access to the specified chat ||
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
- [{#T}](./imbot-chat-user-list.md)
- [{#T}](./imbot-chat-user-delete.md)
- [{#T}](./imbot-chat-leave.md)