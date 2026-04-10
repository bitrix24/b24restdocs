# Get Chat Data im.dialog.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`im`](../scopes/permissions.md)
>
> Who can execute the method: any user — chat participant

The method `im.dialog.get` retrieves information about a chat.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **DIALOG_ID***
[`string`](../data-types.md) | Chat identifier in the format:
- `chatXXX` — chat
- `sgXXX` — group or project chat
- `XXX` — personal chat user identifier

Examples: `chat1435`, `sg103` ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"DIALOG_ID":"chat1435"}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.dialog.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"DIALOG_ID":"chat1435","auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/im.dialog.get
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'im.dialog.get',
            {
                DIALOG_ID: 'chat1435'
            }
        );

        console.log(response.getData().result);
    }
    catch (error)
    {
        console.error(error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'im.dialog.get',
                [
                    'DIALOG_ID' => 'chat1435',
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'im.dialog.get',
        {
            DIALOG_ID: 'chat1435'
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
        'im.dialog.get',
        [
            'DIALOG_ID' => 'chat1435',
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
        "id": 1439,
        "parent_chat_id": 0,
        "parent_message_id": 0,
        "name": "Deal Chat",
        "description": "Discussion about the deal",
        "owner": 503,
        "extranet": false,
        "avatar": "",
        "color": "#f76187",
        "type": "crm",
        "counter": 0,
        "user_counter": 3,
        "message_count": 3,
        "unread_id": 0,
        "restrictions": {
            "avatar": false,
            "rename": false,
            "extend": true,
            "call": true,
            "mute": true,
            "leave": true,
            "leave_owner": false,
            "send": true,
            "user_list": true,
            "path": "",
            "path_title": ""
        },
        "last_message_id": 84477,
        "last_id": 84477,
        "marked_id": 0,
        "disk_folder_id": 0,
        "entity_type": "CRM",
        "entity_id": "DEAL|1663",
        "entity_data_1": "",
        "entity_data_2": "",
        "entity_data_3": "",
        "mute_list": [],
        "date_create": "2026-02-25T16:50:58+01:00",
        "message_type": "C",
        "public": "",
        "role": "owner",
        "entity_link": {
            "type": "DEAL",
            "url": "/crm/deal/details/1663/",
            "id": "DEAL|1663"
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
                "user_id": 103,
                "user_name": "John Smith",
                "message_id": 0,
                "date": null
            },
            {
                "user_id": 547,
                "user_name": "Peter Johnson",
                "message_id": 0,
                "date": null
            }
        ],
        "manager_list": [
            503
        ],
        "last_message_views": {
            "message_id": 84477,
            "first_viewers": [],
            "count_of_viewers": 0
        },
        "dialog_id": "chat1439"
    },
    "time": {
        "start": 1772030091,
        "finish": 1772030091.165223,
        "duration": 0.1652228832244873,
        "processing": 0,
        "date_start": "2026-02-25T17:34:51+01:00",
        "date_finish": "2026-02-25T17:34:51+01:00",
        "operating_reset_at": 1772030691,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | Root object containing chat data [(detailed description)](#result-item) ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

#### Object result-item {#result-item}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Chat identifier ||
|| **parent_chat_id**
[`integer`](../data-types.md) | Parent chat identifier ||
|| **parent_message_id**
[`integer`](../data-types.md) | Parent message identifier ||
|| **name**
[`string`](../data-types.md) | Chat name ||
|| **description**
[`string`](../data-types.md) | Chat description ||
|| **owner**
[`integer`](../data-types.md) | Chat owner identifier ||
|| **extranet**
[`boolean`](../data-types.md) | Indicator of external extranet user participation ||
|| **avatar**
[`string`](../data-types.md) | Link to the chat avatar ||
|| **color**
[`string`](../data-types.md) | Chat color in HEX format ||
|| **type**
[`string`](../data-types.md) | Chat type ||
|| **counter**
[`integer`](../data-types.md) | Unread message counter ||
|| **user_counter**
[`integer`](../data-types.md) | Number of chat participants ||
|| **message_count**
[`integer`](../data-types.md) | Number of messages in the chat ||
|| **unread_id**
[`integer`](../data-types.md) | Identifier of the first unread message ||
|| **restrictions**
[`object`](../data-types.md) | Restrictions on actions in the chat [(detailed description)](#restrictions) ||
|| **last_message_id**
[`integer`](../data-types.md) | Identifier of the last message ||
|| **last_id**
[`integer`](../data-types.md) | Identifier of the last read message ||
|| **marked_id**
[`integer`](../data-types.md) | Identifier of the marked message ||
|| **disk_folder_id**
[`integer`](../data-types.md) | Identifier of the chat folder on Drive ||
|| **entity_type**
[`string`](../data-types.md) | External code of the chat: type ||
|| **entity_id**
[`string`](../data-types.md) | External code of the chat: identifier ||
|| **entity_data_1**
[`string`](../data-types.md) | External data 1 for the chat ||
|| **entity_data_2**
[`string`](../data-types.md) | External data 2 for the chat ||
|| **entity_data_3**
[`string`](../data-types.md) | External data 3 for the chat ||
|| **mute_list**
[`array`](../data-types.md) | List of users with notifications disabled ||
|| **date_create**
[`datetime`](../data-types.md) | Chat creation date in ATOM format ||
|| **message_type**
[`string`](../data-types.md) | Type of chat messages ||
|| **public**
[`string`](../data-types.md) | Indicator of chat public status ||
|| **role**
[`string`](../data-types.md) | Current user's role in the chat ||
|| **entity_link**
[`object`](../data-types.md) | Link to the related object [(detailed description)](#entity-link) ||
|| **text_field_enabled**
[`boolean`](../data-types.md) | Availability of the message input field ||
|| **background_id**
[`integer`](../data-types.md) | Identifier of the chat background. If not specified, the value is `null` ||
|| **permissions**
[`object`](../data-types.md) | Permissions for actions in the chat [(detailed description)](#permissions) ||
|| **is_new**
[`boolean`](../data-types.md) | Indicator of a new dialog ||
|| **readed_list**
[`array`](../data-types.md) | List of users and read statuses [(detailed description)](#readed-list-item) ||
|| **manager_list**
[`array`](../data-types.md) | List of chat manager identifiers ||
|| **last_message_views**
[`object`](../data-types.md) | Information about views of the last message [(detailed description)](#last-message-views) ||
|| **dialog_id**
[`string`](../data-types.md) | Identifier of the dialog passed in the `DIALOG_ID` parameter ||
|#

#### Object restrictions {#restrictions}

#|
|| **Name**
`type` | **Description** ||
|| **avatar**
[`boolean`](../data-types.md) | Availability of avatar change ||
|| **rename**
[`boolean`](../data-types.md) | Availability of name change ||
|| **extend**
[`boolean`](../data-types.md) | Availability of chat extension ||
|| **call**
[`boolean`](../data-types.md) | Availability of calls ||
|| **mute**
[`boolean`](../data-types.md) | Availability of notifications mute ||
|| **leave**
[`boolean`](../data-types.md) | Availability of leaving the chat ||
|| **leave_owner**
[`boolean`](../data-types.md) | Availability of owner leaving the chat ||
|| **send**
[`boolean`](../data-types.md) | Availability of message sending ||
|| **user_list**
[`boolean`](../data-types.md) | Availability of viewing the participant list ||
|| **path**
[`string`](../data-types.md) | Link to the chat ||
|| **path_title**
[`string`](../data-types.md) | Text of the link to the chat ||
|#

#### Object entity_link {#entity-link}

#|
|| **Name**
`type` | **Description** ||
|| **type**
[`string`](../data-types.md) | Type of the related object ||
|| **url**
[`string`](../data-types.md) | Link to the related object ||
|| **id**
[`string`](../data-types.md) | Identifier of the related object ||
|#

#### Object permissions {#permissions}

#|
|| **Name**
`type` | **Description** ||
|| **manage_users_add**
[`string`](../data-types.md) | Permission to add participants ||
|| **manage_users_delete**
[`string`](../data-types.md) | Permission to remove participants ||
|| **manage_ui**
[`string`](../data-types.md) | Permission to manage the chat interface ||
|| **manage_settings**
[`string`](../data-types.md) | Permission to manage chat settings ||
|| **manage_messages**
[`string`](../data-types.md) | Permission to manage messages ||
|| **can_post**
[`string`](../data-types.md) | Permission to send messages ||
|#

#### Object readed_list {#readed-list-item}

#|
|| **Name**
`type` | **Description** ||
|| **user_id**
[`integer`](../data-types.md) | User identifier ||
|| **user_name**
[`string`](../data-types.md) | User name ||
|| **message_id**
[`integer`](../data-types.md) | Identifier of the last read message ||
|| **date**
[`datetime`](../data-types.md) | Read date. If not specified, the value is `null` ||
|#

#### Object last_message_views {#last-message-views}

#|
|| **Name**
`type` | **Description** ||
|| **message_id**
[`integer`](../data-types.md) | Identifier of the message ||
|| **first_viewers**
[`array`](../data-types.md) | List of first viewers ||
|| **count_of_viewers**
[`integer`](../data-types.md) | Number of views ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "DIALOG_ID_EMPTY",
    "error_description": "Dialog ID can't be empty"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `DIALOG_ID_EMPTY` | Dialog ID can't be empty | Required parameter `DIALOG_ID` not provided ||
|| `403` | `ACCESS_ERROR` | You do not have access to the specified dialog | Insufficient rights to view the dialog ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./im-chat-add.md)
- [{#T}](./im-chat-get.md)
- [{#T}](./im-recent-get.md)
- [{#T}](./im-recent-list.md)