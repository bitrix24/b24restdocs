# Find Chats im.search.chat.list

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.search.chat.list` performs a search for chats that the current user has access to. The search is conducted based on the title, first name, and last name of chat participants.

Results are sorted in descending order of identifiers.

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **FIND**
[`string`](../../data-types.md) | Search phrase for querying indexed chat data. The minimum number of characters for a search is `3` ||
|| **FIND_LINES**
[`string`](../../data-types.md) | Search phrase for finding chats among Open Channels. The minimum number of characters for a search is `3` ||
|| **OFFSET**
[`integer`](../../data-types.md) | Offset for the chat sample. Default is `0` ||
|| **LIMIT**
[`integer`](../../data-types.md) | Number of items in the sample. Default is `10`. Maximum value is `50` ||
|#

{% note info "" %}

At least one parameter must be provided: `FIND` or `FIND_LINES`.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"FIND":"Project","FIND_LINES":"Line","OFFSET":0,"LIMIT":10}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.search.chat.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"FIND":"Project","FIND_LINES":"Line","OFFSET":0,"LIMIT":10,"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/im.search.chat.list
    ```

- JS

    ```js
    try {
      const response = await $b24.callMethod('im.search.chat.list', {
        FIND: 'Project',
        FIND_LINES: 'Line',
        OFFSET: 0,
        LIMIT: 10,
      });

      const { result, total, next } = response.getData();
      console.log(result, total, next);
    } catch (error) {
      console.error(error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service->core->call(
            'im.search.chat.list',
            [
                'FIND' => 'Project',
                'FIND_LINES' => 'Line',
                'OFFSET' => 0,
                'LIMIT' => 10,
            ]
        );

        $result = $response->getResponseData()->getResult();

        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            var_dump($result->data());
        }
    } catch (Throwable $exception) {
        echo $exception->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'im.search.chat.list',
        {
            FIND: 'Project',
            FIND_LINES: 'Line',
            OFFSET: 0,
            LIMIT: 10,
        },
        function(result) {
            if (result.error()) {
                console.error(result.error().ex);
            } else {
                console.log(result.data(), result.total(), result.next());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'im.search.chat.list',
        [
            'FIND' => 'Project',
            'FIND_LINES' => 'Line',
            'OFFSET' => 0,
            'LIMIT' => 10,
        ]
    );

    if (!empty($result['error'])) {
        echo 'Error: ' . $result['error_description'];
    } else {
        var_dump($result['result']);
    }
    ```
{% endlist %}

## Response Handling

HTTP Code: **200**

```json
{
    "result": [
        {
            "id": 1137,
            "parent_chat_id": 0,
            "parent_message_id": 0,
            "name": "Project: \"template development\"",
            "description": null,
            "owner": 27,
            "extranet": false,
            "avatar": "",
            "color": "#3e99ce",
            "type": "sonetGroup",
            "counter": 0,
            "user_counter": 2,
            "message_count": 4,
            "unread_id": 0,
            "restrictions": {
                "avatar": false,
                "rename": false,
                "extend": false,
                "call": true,
                "mute": true,
                "leave": false,
                "leave_owner": false,
                "send": true,
                "user_list": true,
                "path": "/workgroups/group/#ID#/",
                "path_title": "Go to group"
            },
            "last_message_id": 80461,
            "last_id": 80461,
            "marked_id": 0,
            "disk_folder_id": 0,
            "entity_type": "SONET_GROUP",
            "entity_id": "121",
            "entity_data_1": "",
            "entity_data_2": "",
            "entity_data_3": "",
            "mute_list": [],
            "date_create": "2024-07-26T15:28:02+02:00",
            "message_type": "C",
            "public": "",
            "role": "owner",
            "entity_link": {
                "type": "SONET_GROUP",
                "url": "/workgroups/group/121/",
                "id": "121"
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
            "is_new": false
        },
        ... // description for each chat
    ],
    "total": 2,
    "time": {
        "start": 1772645157,
        "finish": 1772645157.558317,
        "duration": 0.5583169460296631,
        "processing": 0,
        "date_start": "2026-03-04T20:25:57+02:00",
        "date_finish": "2026-03-04T20:25:57+02:00",
        "operating_reset_at": 1772645757,
        "operating": 0
    }
}
```

## Returned Data

#|
|| **Name**
`Type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | List of found chats.

The structure of the chat object is described in detail [below](#chat-object) ||
|| **total**
[`integer`](../../data-types.md) | Total number of found chats ||
|| **next**
[`integer`](../../data-types.md) | Offset for the next page. This field is returned if there is a next page ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

### Chat Object {#chat-object}

#|
|| **Name**
`Type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Identifier of the chat ||
|| **parent_chat_id**
[`integer`](../../data-types.md) | Identifier of the parent chat ||
|| **parent_message_id**
[`integer`](../../data-types.md) | Identifier of the parent message ||
|| **name**
[`string`](../../data-types.md) | Name of the chat ||
|| **description**
[`string`](../../data-types.md)
[`null`](../../data-types.md) | Description of the chat ||
|| **owner**
[`integer`](../../data-types.md) | Identifier of the chat owner ||
|| **extranet**
[`boolean`](../../data-types.md) | Indicates participation of extranet users ||
|| **avatar**
[`string`](../../data-types.md)
[`null`](../../data-types.md) | Link to the chat avatar ||
|| **color**
[`string`](../../data-types.md) | Color of the chat in HEX format ||
|| **type**
[`string`](../../data-types.md) | Type of the chat ||
|| **counter**
[`integer`](../../data-types.md) | Value of the unread message counter for the current user ||
|| **user_counter**
[`integer`](../../data-types.md) | Number of participants in the chat ||
|| **message_count**
[`integer`](../../data-types.md) | Number of messages in the chat ||
|| **unread_id**
[`integer`](../../data-types.md) | Identifier of the first unread message ||
|| **restrictions**
[`object`](../../data-types.md) | Restrictions on actions in the chat.

The structure of the object is described in detail [below](#restrictions-object) ||
|| **last_message_id**
[`integer`](../../data-types.md) | Identifier of the last message ||
|| **last_id**
[`integer`](../../data-types.md) | Identifier of the last message in the chat marked as read by the current user ||
|| **marked_id**
[`integer`](../../data-types.md) | Identifier of the marked message ||
|| **disk_folder_id**
[`integer`](../../data-types.md) | Identifier of the Drive folder associated with the chat ||
|| **entity_type**
[`string`](../../data-types.md) | Type of the object to which the chat is linked ||
|| **entity_id**
[`string`](../../data-types.md) | Identifier of the object to which the chat is linked ||
|| **entity_data_1**
[`string`](../../data-types.md) | Additional data of the chat object — field 1 ||
|| **entity_data_2**
[`string`](../../data-types.md) | Additional data of the chat object — field 2 ||
|| **entity_data_3**
[`string`](../../data-types.md) | Additional data of the chat object — field 3 ||
|| **mute_list**
[`array`](../../data-types.md)
[`object`](../../data-types.md) | List of users with notifications turned off ||
|| **date_create**
[`string`](../../data-types.md) | Creation date of the chat in ISO 8601 format (RFC3339) ||
|| **message_type**
[`string`](../../data-types.md) | Type of the chat message from the `TYPE` field of the chat table ||
|| **public**
[`object`](../../data-types.md)
[`string`](../../data-types.md) | Public data of the chat.

The structure of the object is described in detail [below](#public-object) ||
|| **role**
[`string`](../../data-types.md) | Role of the current user in the chat ||
|| **entity_link**
[`object`](../../data-types.md) | Link to the related chat object.

The structure of the object is described in detail [below](#entity-link-object) ||
|| **text_field_enabled**
[`boolean`](../../data-types.md) | Indicates availability of the text input field ||
|| **background_id**
[`integer`](../../data-types.md)
[`null`](../../data-types.md) | Identifier of the chat background ||
|| **permissions**
[`object`](../../data-types.md) | Permissions of the current user.

The structure of the object is described in detail [below](#permissions-object) ||
|| **is_new**
[`boolean`](../../data-types.md) | Indicates if the chat is new ||
|#

### Restrictions Object {#restrictions-object}

#|
|| **Name**
`Type` | **Description** ||
|| **avatar**
[`boolean`](../../data-types.md) | Allowed to change the chat avatar ||
|| **rename**
[`boolean`](../../data-types.md) | Allowed to change the chat name ||
|| **extend**
[`boolean`](../../data-types.md) | Allowed to extend the functionality of the chat ||
|| **call**
[`boolean`](../../data-types.md) | Calls in the chat are allowed ||
|| **mute**
[`boolean`](../../data-types.md) | Allowed to mute chat notifications ||
|| **leave**
[`boolean`](../../data-types.md) | Allowed to leave the chat ||
|| **leave_owner**
[`boolean`](../../data-types.md) | Allowed for the owner to leave the chat ||
|| **send**
[`boolean`](../../data-types.md) | Allowed to send messages ||
|| **user_list**
[`boolean`](../../data-types.md) | Access to view the list of participants ||
|| **path**
[`string`](../../data-types.md) | System path to navigate to the related object, for example, to the workgroup ||
|| **path_title**
[`string`](../../data-types.md) | Link text for navigating via `path`. This field is available along with `path` ||
|#

### Public Object {#public-object}

#|
|| **Name**
`Type` | **Description** ||
|| **code**
[`string`](../../data-types.md) | Public code of the chat ||
|| **link**
[`string`](../../data-types.md) | Public link to the chat ||
|#

### Entity Link Object {#entity-link-object}

#|
|| **Name**
`Type` | **Description** ||
|| **type**
[`string`](../../data-types.md) | Type of the related object ||
|| **url**
[`string`](../../data-types.md) | URL of the related object ||
|| **id**
[`string`](../../data-types.md)
[`integer`](../../data-types.md) | Identifier of the related object ||
|#

### Permissions Object {#permissions-object}

#|
|| **Name**
`Type` | **Description** ||
|| **manage_users_add**
[`string`](../../data-types.md) | Permission to add users ||
|| **manage_users_delete**
[`string`](../../data-types.md) | Permission to delete users ||
|| **manage_ui**
[`string`](../../data-types.md) | Permission to manage interface settings ||
|| **manage_settings**
[`string`](../../data-types.md) | Permission to manage chat settings ||
|| **manage_messages**
[`string`](../../data-types.md) | Permission to manage messages ||
|| **can_post**
[`string`](../../data-types.md) | Permission to send messages ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "FIND_SHORT",
    "error_description": "Too short a search phrase."
}
```

{% include notitle [Error Handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `FIND_SHORT` | Too short a search phrase | No search parameter was provided or the phrase is less than three characters ||
|#

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./im-search-department-list.md)
- [{#T}](./im-search-user-list.md)
- [{#T}](./im-search-last-add.md)
- [{#T}](./im-search-last-get.md)
- [{#T}](./im-search-last-delete.md)