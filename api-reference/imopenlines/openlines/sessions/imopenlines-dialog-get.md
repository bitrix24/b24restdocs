# Get Information About the Operator's Dialog imopenlines.dialog.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`imopenlines`](../../../scopes/permissions.md)
>
> Who can execute the method: any user with access permission to the dialog

The method `imopenlines.dialog.get` returns data from the open line chat. You only need to provide one of the parameters.

## Method Parameters

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CHAT_ID**
[`integer`](../../../data-types.md) | Identifier of the open line chat.

The identifier can be obtained using the [imopenlines.session.open](./imopenlines-session-open.md) or [imopenlines.session.history.get](./imopenlines-session-history-get.md) methods. ||
|| **DIALOG_ID**
[`string`](../../../data-types.md) | Identifier of the dialog in the format `chat<ID>`, where `<ID>` is the identifier of the open line chat. ||
|| **SESSION_ID**
[`integer`](../../../data-types.md) | Identifier of the session. 

The identifier can be obtained using the [imopenlines.session.history.get](./imopenlines-session-history-get.md) method in the `sessionId` field. ||
|| **USER_CODE**
[`string`](../../../data-types.md) | String code of the user for the external system channel. 

Code format: ```<connector>|<LINE_ID>|<CONNECTOR_CHAT_ID>|<CONNECTOR_USER_ID>```, where:
- `<connector>` — identifier of the connector: `livechat`, `telegram`, and others
- `<LINE_ID>` — identifier of the open line
- `<CONNECTOR_CHAT_ID>` — identifier of the chat in the channel
- `<CONNECTOR_USER_ID>` — identifier of the user in the channel

The value can be obtained using the [imopenlines.session.history.get](./imopenlines-session-history-get.md) method from `result.chat.<chatId>.entityId`. ||
|#

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"USER_CODE":"livechat|1|1373|211"}' \
      https://your-domain.bitrix24.com/rest/1/webhook_key/imopenlines.dialog.get.json
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"USER_CODE":"livechat|1|1373|211","auth":"<access_token>"}' \
      https://your-domain.bitrix24.com/rest/imopenlines.dialog.get.json
    ```

- JS

    ```js
    try {
        const response = await $b24.callMethod(
            'imopenlines.dialog.get',
            {
                USER_CODE: 'livechat|1|1373|211',
            }
        );

        const { result } = response.getData();
        console.log(result);
    } catch (error) {
        console.error(error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'imopenlines.dialog.get',
                [
                    'USER_CODE' => 'livechat|1|1373|211',
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    } catch (Throwable $exception) {
        error_log($exception->getMessage());
        echo 'Error getting dialog: ' . $exception->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'imopenlines.dialog.get',
        {
            USER_CODE: 'livechat|1|1373|211',
        },
        function(result) {
            if (result.error()) {
                console.error(result.error().ex);
            } else {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'imopenlines.dialog.get',
        [
            'USER_CODE' => 'livechat|1|1373|211',
        ]
    );

    if (!empty($result['error'])) {
        echo 'Error: ' . $result['error_description'];
    } else {
        echo 'Success: ' . print_r($result['result'], true);
    }
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "id": 1777,
        "parent_chat_id": 0,
        "parent_message_id": 0,
        "name": "Green Guest #17 - Bitrix24 Documentation",
        "description": null,
        "owner": 27,
        "extranet": false,
        "avatar": "",
        "color": "#58cc47",
        "type": "lines",
        "counter": 0,
        "user_counter": 2,
        "message_count": 104,
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
        "last_message_id": 86313,
        "last_id": 86313,
        "marked_id": 0,
        "disk_folder_id": 0,
        "entity_type": "LINES",
        "entity_id": "livechat|22|1775|599",
        "entity_data_1": "Y|LEAD|1209|N|N|343|1773682918|0|0|0",
        "entity_data_2": "LEAD|1209|COMPANY|0|CONTACT|0|DEAL|0",
        "entity_data_3": "N",
        "mute_list": [],
        "date_create": "2026-03-13T16:50:15+01:00",
        "message_type": "L",
        "public": "",
        "role": "owner",
        "entity_link": {
            "type": "LINES",
            "url": "",
            "id": "livechat|22|1775|599"
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
                "user_id": 599,
                "user_name": "Guest",
                "message_id": 86101,
                "date": null
            }
        ],
        "manager_list": [27],
        "last_message_views": {
            "message_id": 86313,
            "first_viewers": [
                {
                    "user_id": 27,
                    "user_name": "Samantha Johnson",
                    "date": "2026-03-16T20:50:37+01:00"
                }
            ],
            "count_of_viewers": 0
        },
        "dialog_id": "chat1777"
    },
    "time": {
        "start": 1773683678,
        "finish": 1773683678.423382,
        "duration": 0.423382043838501,
        "processing": 0,
        "date_start": "2026-03-16T20:54:38+01:00",
        "date_finish": "2026-03-16T20:54:38+01:00",
        "operating_reset_at": 1773684278,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Chat data object [(detailed description)](#result) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

### Result Object {#result}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Identifier of the chat ||
|| **parent_chat_id**
[`integer`](../../../data-types.md) | Identifier of the parent chat ||
|| **parent_message_id**
[`integer`](../../../data-types.md) | Identifier of the parent message ||
|| **name**
[`string`](../../../data-types.md) | Name of the chat ||
|| **description**
[`string`](../../../data-types.md) | Description of the chat or `null` ||
|| **owner**
[`integer`](../../../data-types.md) | Identifier of the chat owner ||
|| **extranet**
[`boolean`](../../../data-types.md) | Indicator of an extranet chat ||
|| **avatar**
[`string`](../../../data-types.md) | URL of the chat avatar or an empty string ||
|| **color**
[`string`](../../../data-types.md) | Color of the chat in HEX format ||
|| **dialog_id**
[`string`](../../../data-types.md) | Identifier of the dialog in the format `chat<ID>` ||
|| **type**
[`string`](../../../data-types.md) | Type of the chat, for open lines the value is `lines` ||
|| **counter**
[`integer`](../../../data-types.md) | Number of unread messages for the current user ||
|| **user_counter**
[`integer`](../../../data-types.md) | Number of participants with unread messages ||
|| **message_count**
[`integer`](../../../data-types.md) | Total number of messages in the chat ||
|| **unread_id**
[`integer`](../../../data-types.md) | Identifier of the first unread message or `0` ||
|| **restrictions**
[`object`](../../../data-types.md) | Permissions for actions with the chat [(detailed description)](#restrictions) ||
|| **last_message_id**
[`integer`](../../../data-types.md) | Identifier of the last message ||
|| **last_id**
[`integer`](../../../data-types.md) | Service identifier of the last message ||
|| **marked_id**
[`integer`](../../../data-types.md) | Identifier of the marked message or `0` ||
|| **disk_folder_id**
[`integer`](../../../data-types.md) | Identifier of the Drive folder for chat files ||
|| **entity_type**
[`string`](../../../data-types.md) | Type of the chat channel, for open lines the value is `LINES` ||
|| **entity_id**
[`string`](../../../data-types.md) | User code of the open line in the format ```<connector>|<LINE_ID>|<CONNECTOR_CHAT_ID>|<CONNECTOR_USER_ID>```, where:
- `<connector>` — identifier of the connector: `livechat`, `telegram`, and others
- `<LINE_ID>` — identifier of the open line
- `<CONNECTOR_CHAT_ID>` — identifier of the chat in the channel
- `<CONNECTOR_USER_ID>` — identifier of the user in the channel ||
|| **entity_data_1**
[`string`](../../../data-types.md) | String with session data of the open line ||
|| **entity_data_2**
[`string`](../../../data-types.md) | String with CRM bindings ||
|| **entity_data_3**
[`string`](../../../data-types.md) | Additional service flag ||
|| **mute_list**
[`array`](../../../data-types.md) | List of user IDs with notifications turned off ||
|| **date_create**
[`datetime`](../../../data-types.md) | Date and time of chat creation in ISO 8601 format (RFC3339) ||
|| **message_type**
[`string`](../../../data-types.md) | Type of messages in the chat ||
|| **public**
[`string`](../../../data-types.md) | Public flag of the chat ||
|| **role**
[`string`](../../../data-types.md) | Role of the current user in the chat ||
|| **entity_link**
[`object`](../../../data-types.md) | Link to the external system channel [(detailed description)](#entity-link) ||
|| **text_field_enabled**
[`boolean`](../../../data-types.md) | Whether the message input field is available ||
|| **background_id**
[`integer`](../../../data-types.md) | Identifier of the chat background or `null` ||
|| **permissions**
[`object`](../../../data-types.md) | Permissions of the current user in the chat [(detailed description)](#permissions) ||
|| **is_new**
[`boolean`](../../../data-types.md) | Indicator of a new chat ||
|| **readed_list**
[`array`](../../../data-types.md) | List of data about message readings [(detailed description)](#readed-list) ||
|| **manager_list**
[`array`](../../../data-types.md) | List of identifiers of operators assigned as managers ||
|| **last_message_views**
[`object`](../../../data-types.md) | Data about the views of the last message [(detailed description)](#last-message-views) ||
|#

### Restrictions Object {#restrictions}

#|
|| **Name**
`type` | **Description** ||
|| **avatar**
[`boolean`](../../../data-types.md) | Permission to change the chat avatar ||
|| **rename**
[`boolean`](../../../data-types.md) | Permission to change the chat name ||
|| **extend**
[`boolean`](../../../data-types.md) | Permission to extend chat settings ||
|| **call**
[`boolean`](../../../data-types.md) | Permission for calls in the chat ||
|| **mute**
[`boolean`](../../../data-types.md) | Permission to turn off notifications ||
|| **leave**
[`boolean`](../../../data-types.md) | Permission to leave the chat ||
|| **leave_owner**
[`boolean`](../../../data-types.md) | Permission for the chat owner to leave ||
|| **send**
[`boolean`](../../../data-types.md) | Permission to send messages ||
|| **user_list**
[`boolean`](../../../data-types.md) | Permission to view the list of participants ||
|#

### Entity Link Object {#entity-link}

#|
|| **Name**
`type` | **Description** ||
|| **type**
[`string`](../../../data-types.md) | Type of the channel, for open lines the value is `LINES` ||
|| **url**
[`string`](../../../data-types.md) | Link to the external object of the channel or an empty string ||
|| **id**
[`string`](../../../data-types.md) | External identifier of the dialog in the channel ||
|#

### Permissions Object {#permissions}

#|
|| **Name**
`type` | **Description** ||
|| **manage_users_add**
[`string`](../../../data-types.md) | Permission to add participants ||
|| **manage_users_delete**
[`string`](../../../data-types.md) | Permission to remove participants ||
|| **manage_ui**
[`string`](../../../data-types.md) | Permission to manage the chat interface ||
|| **manage_settings**
[`string`](../../../data-types.md) | Permission to manage chat settings ||
|| **manage_messages**
[`string`](../../../data-types.md) | Permission to manage messages ||
|| **can_post**
[`string`](../../../data-types.md) | Permission to send messages ||
|#

### Readed List Item Object {#readed-list}

#|
|| **Name**
`type` | **Description** ||
|| **user_id**
[`integer`](../../../data-types.md) | Identifier of the user ||
|| **user_name**
[`string`](../../../data-types.md) | Name of the user ||
|| **message_id**
[`integer`](../../../data-types.md) | Identifier of the last read message ||
|| **date**
[`datetime`](../../../data-types.md) | Date and time of reading in ISO 8601 format (RFC3339) or `null` ||
|#

### Last Message Views Object {#last-message-views}

#|
|| **Name**
`type` | **Description** ||
|| **message_id**
[`integer`](../../../data-types.md) | Identifier of the message for which view statistics are collected ||
|| **first_viewers**
[`array`](../../../data-types.md) | List of users who viewed the message first [(detailed description)](#first-viewer-item) ||
|| **count_of_viewers**
[`integer`](../../../data-types.md) | Number of other users who viewed the message ||
|#

### First Viewer Item Object {#first-viewer-item}

#|
|| **Name**
`type` | **Description** ||
|| **user_id**
[`integer`](../../../data-types.md) | Identifier of the user ||
|| **user_name**
[`string`](../../../data-types.md) | Name of the user ||
|| **date**
[`datetime`](../../../data-types.md) | Date and time of viewing in ISO 8601 format (RFC3339) ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ACCESS_ERROR",
    "error_description": "You do not have access to the specified dialog"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `ACCESS_ERROR` | You do not have access to the specified dialog | Dialog not found or no access to it ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./imopenlines-session-open.md)
- [{#T}](./imopenlines-session-start.md)
- [{#T}](./imopenlines-session-join.md)
- [{#T}](./imopenlines-session-history-get.md)
- [{#T}](./imopenlines-session-intercept.md)
- [{#T}](./imopenlines-session-mode-pin.md)
- [{#T}](./imopenlines-session-mode-pin-all.md)
- [{#T}](./imopenlines-session-mode-unpin-all.md)
- [{#T}](./imopenlines-session-mode-silent.md)
- [{#T}](./imopenlines-session-head-vote.md)
- [{#T}](./imopenlines-message-session-start.md)
- [{#T}](./imopenlines-crm-lead-create.md)