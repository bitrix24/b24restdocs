# Get a Shortened List of Recent Chats im.recent.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`im`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.recent.get` retrieves a list of the user's recent chats.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **SKIP_OPENLINES**
[`string`](../data-types.md) | Skip chats from Open Channels.

Possible values:
- `Y` — yes
- `N` — no ||
|| **SKIP_CHAT**
[`string`](../data-types.md) | Skip group chats.

Possible values:
- `Y` — yes
- `N` — no ||
|| **SKIP_DIALOG**
[`string`](../data-types.md) | Skip one-on-one dialogs.

Possible values:
- `Y` — yes
- `N` — no ||
|| **LAST_UPDATE**
[`datetime`](../data-types.md) | Retrieve data from the specified date in [ATOM](https://www.php.net/manual/en/class.datetimeinterface.php#datetimeinterface.constants.atom) (ISO-8601) format ||
|| **ONLY_OPENLINES**
[`string`](../data-types.md) | Select only chats from Open Channels.

Possible values:
- `Y` — yes
- `N` — no ||
|| **LAST_SYNC_DATE**
[`datetime`](../data-types.md) | Date of the previous retrieval in [ATOM](https://www.php.net/manual/en/class.datetimeinterface.php#datetimeinterface.constants.atom) (ISO-8601) format to load changes that occurred in the list since the specified date.

The retrieval returns data no older than 7 days ||
|#

## Code Examples

{% include [Examples Note](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"SKIP_OPENLINES":"Y","LAST_UPDATE":"2026-02-25T18:30:00+01:00"}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.recent.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"SKIP_OPENLINES":"Y","LAST_UPDATE":"2026-02-25T18:30:00+01:00","auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/im.recent.get
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'im.recent.get',
            {
                SKIP_OPENLINES: 'Y',
                LAST_UPDATE: '2026-02-25T18:30:00+01:00'
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
                'im.recent.get',
                [
                    'SKIP_OPENLINES' => 'Y',
                    'LAST_UPDATE' => '2026-02-25T18:30:00+01:00',
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
        'im.recent.get',
        {
            SKIP_OPENLINES: 'Y',
            LAST_UPDATE: '2026-02-25T18:30:00+01:00'
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
        'im.recent.get',
        [
            'SKIP_OPENLINES' => 'Y',
            'LAST_UPDATE' => '2026-02-25T18:30:00+01:00',
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
    "result": [
        {
            "id": "chat1451",
            "chat_id": 1451,
            "type": "chat",
            "avatar": {
                "url": "",
                "color": "#df532d"
            },
            "title": "Maximally Complete Task Template",
            "message": {
                "id": 84501,
                "text": "John Doe created a task [Attachment]",
                "file": false,
                "author_id": 0,
                "attach": true,
                "sticker": null,
                "date": "2026-02-26T00:01:26+03:00",
                "status": "received",
                "uuid": null
            },
            "counter": 0,
            "last_id": 84501,
            "pinned": false,
            "unread": false,
            "has_reminder": false,
            "date_update": "2026-02-26T00:01:26+03:00",
            "date_last_activity": "2026-02-26T00:01:26+03:00",
            "chat": {
                "id": 1451,
                "parent_chat_id": 0,
                "parent_message_id": 0,
                "name": "Maximally Complete Task Template",
                "owner": 503,
                "extranet": false,
                "contains_collaber": false,
                "avatar": "",
                "color": "#df532d",
                "type": "tasksTask",
                "entity_type": "TASKS_TASK",
                "entity_id": "8293",
                "entity_data_1": "",
                "entity_data_2": "",
                "entity_data_3": "",
                "mute_list": [],
                "manager_list": [
                    503
                ],
                "date_create": "2026-02-26T00:01:26+03:00",
                "message_type": "X",
                "user_counter": 4,
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
                "role": "OWNER",
                "text_field_enabled": true,
                "background_id": null,
                "entity_link": {
                    "type": "TASKS",
                    "url": "/company/personal/user/503/tasks/task/view/8293/?ta_sec=chat_tasks&ta_el=view_button",
                    "id": "8293"
                },
                "permissions": {
                    "manage_users_add": "member",
                    "manage_users_delete": "manager",
                    "manage_ui": "member",
                    "manage_settings": "owner",
                    "manage_messages": "member",
                    "can_post": "member"
                },
                "public": ""
            },
            "user": {
                "id": 0
            },
            "options": []
        },
        {
            "id": "chat1449",
            "chat_id": 1449,
            "type": "chat",
            "avatar": {
                "url": "",
                "color": "#ab7761"
            },
            "title": "Maximally Complete Task Template",
            "message": {
                "id": 84499,
                "text": "John Doe created a task [Attachment]",
                "file": false,
                "author_id": 0,
                "attach": true,
                "sticker": null,
                "date": "2026-02-26T00:01:25+03:00",
                "status": "received",
                "uuid": null
            },
            "counter": 0,
            "last_id": 84499,
            "pinned": false,
            "unread": false,
            "has_reminder": false,
            "date_update": "2026-02-26T00:01:25+03:00",
            "date_last_activity": "2026-02-26T00:01:25+03:00",
            "chat": {
                "id": 1449,
                "parent_chat_id": 0,
                "parent_message_id": 0,
                "name": "Maximally Complete Task Template",
                "owner": 503,
                "extranet": false,
                "contains_collaber": false,
                "avatar": "",
                "color": "#ab7761",
                "type": "tasksTask",
                "entity_type": "TASKS_TASK",
                "entity_id": "8291",
                "entity_data_1": "",
                "entity_data_2": "",
                "entity_data_3": "",
                "mute_list": [],
                "manager_list": [
                    503
                ],
                "date_create": "2026-02-26T00:01:25+03:00",
                "message_type": "X",
                "user_counter": 4,
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
                "role": "OWNER",
                "text_field_enabled": true,
                "background_id": null,
                "entity_link": {
                    "type": "TASKS",
                    "url": "/company/personal/user/503/tasks/task/view/8291/?ta_sec=chat_tasks&ta_el=view_button",
                    "id": "8291"
                },
                "permissions": {
                    "manage_users_add": "member",
                    "manage_users_delete": "manager",
                    "manage_ui": "member",
                    "manage_settings": "owner",
                    "manage_messages": "member",
                    "can_post": "member"
                },
                "public": ""
            },
            "user": {
                "id": 0
            },
            "options": []
        }
    ],
    "time": {
        "start": 1772086038,
        "finish": 1772086038.652287,
        "duration": 0.6522870063781738,
        "processing": 0,
        "date_start": "2026-02-26T09:07:18+03:00",
        "date_finish": "2026-02-26T09:07:18+03:00",
        "operating_reset_at": 1772086638,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../data-types.md) | List of recent dialogs [(detailed description)](#result-item) ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

#### Object result-item {#result-item}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../data-types.md) | Identifier of the dialog: number for user, `chatXXX` for chat ||
|| **type**
[`string`](../data-types.md) | Type of record: `user` for user, `chat` for chat ||
|| **avatar**
[`object`](../data-types.md) | Object describing the avatar of the record [(detailed description)](#avatar) ||
|| **title**
[`string`](../data-types.md) | Title of the record: first and last name for user, chat name for chat ||
|| **message**
[`object`](../data-types.md) | Object describing the last message [(detailed description)](#message) ||
|| **counter**
[`integer`](../data-types.md) | Unread message counter ||
|| **chat_id**
[`integer`](../data-types.md) | Chat identifier ||
|| **last_id**
[`integer`](../data-types.md) | Identifier of the last read message ||
|| **pinned**
[`boolean`](../data-types.md) | Indicator of a pinned dialog ||
|| **unread**
[`boolean`](../data-types.md) | Indicator of a manual "unread" mark ||
|| **has_reminder**
[`boolean`](../data-types.md) | Indicator of a set reminder ||
|| **date_update**
[`datetime`](../data-types.md) | Date of the record update in the recent list in ATOM format ||
|| **date_last_activity**
[`datetime`](../data-types.md) | Date of the last activity in the dialog in ATOM format ||
|| **user**
[`object`](../data-types.md) | Object describing the user. Not available for records of type `chat`. [(detailed description)](#user) ||
|| **chat**
[`object`](../data-types.md) | Object describing the chat. Not available for records of type `user`. [(detailed description)](#chat) ||
|| **options**
[`array`](../data-types.md) | Additional parameters of the record ||
|#

#### Object avatar {#avatar}

#|
|| **Name**
`type` | **Description** ||
|| **url**
[`string`](../data-types.md) | Link to the avatar. If empty, the avatar is not set ||
|| **color**
[`string`](../data-types.md) | Color of the dialog in HEX format ||
|#

#### Object message {#message}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Identifier of the message ||
|| **text**
[`string`](../data-types.md) | Text of the message without BB codes and line breaks ||
|| **file**
[`boolean`](../data-types.md) | Indicator of the presence of files ||
|| **attach**
[`boolean`](../data-types.md) | Indicator of the presence of attachments ||
|| **author_id**
[`integer`](../data-types.md) | Identifier of the message author ||
|| **date**
[`datetime`](../data-types.md) | Date of the message in ATOM format ||
|| **sticker**
[`integer`](../data-types.md) | Identifier of the sticker. If there is no sticker, the value is `null` ||
|| **status**
[`string`](../data-types.md) | Delivery status of the message ||
|| **uuid**
[`string`](../data-types.md) | External identifier of the message. If not set, the value is `null` ||
|#

#### Object user {#user}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Identifier of the user ||
|| **name**
[`string`](../data-types.md) | User's full name ||
|| **first_name**
[`string`](../data-types.md) | User's first name ||
|| **last_name**
[`string`](../data-types.md) | User's last name ||
|| **work_position**
[`string`](../data-types.md) | User's job title ||
|| **color**
[`string`](../data-types.md) | User's color in HEX format ||
|| **avatar**
[`string`](../data-types.md) | Link to the avatar. If empty, the avatar is not set ||
|| **gender**
[`string`](../data-types.md) | User's gender ||
|| **birthday**
[`string`](../data-types.md) | Birthday in `DD-MM` format. If empty, not set ||
|| **extranet**
[`boolean`](../data-types.md) | Indicator of an external extranet user ||
|| **network**
[`boolean`](../data-types.md) | Indicator of a Bitrix24.Network user ||
|| **bot**
[`boolean`](../data-types.md) | Indicator of a bot ||
|| **connector**
[`boolean`](../data-types.md) | Indicator of a user from Open Channels ||
|| **external_auth_id**
[`string`](../data-types.md) | External authorization code ||
|| **status**
[`string`](../data-types.md) | Selected status of the user ||
|| **idle**
[`datetime`](../data-types.md) | Date when the user stepped away from the computer, in ATOM format. If not set, `false` ||
|| **last_activity_date**
[`datetime`](../data-types.md) | Date of the user's last action in ATOM format ||
|| **mobile_last_date**
[`datetime`](../data-types.md) | Date of the last action in the mobile application in ATOM format. If not set, `false` ||
|| **absent**
[`datetime`](../data-types.md) | Date until which the user is on vacation, in ATOM format. If not set, `false` ||
|#

#### Object chat {#chat}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Identifier of the chat ||
|| **title**
[`string`](../data-types.md) | Title of the chat ||
|| **name**
[`string`](../data-types.md) | Name of the chat (field from the response) ||
|| **owner**
[`integer`](../data-types.md) | Identifier of the chat owner ||
|| **extranet**
[`boolean`](../data-types.md) | Indicator of the participation of an external extranet user in the chat ||
|| **parent_chat_id**
[`integer`](../data-types.md) | Identifier of the parent chat ||
|| **parent_message_id**
[`integer`](../data-types.md) | Identifier of the parent message ||
|| **contains_collaber**
[`boolean`](../data-types.md) | Indicator of the participation of collaborator users ||
|| **color**
[`string`](../data-types.md) | Color of the chat in HEX format ||
|| **avatar**
[`string`](../data-types.md) | Link to the avatar. If empty, the avatar is not set ||
|| **type**
[`string`](../data-types.md) | Type of chat: group, call, open line, etc. ||
|| **entity_type**
[`string`](../data-types.md) | External code for the chat: type ||
|| **entity_id**
[`string`](../data-types.md) | External code for the chat: identifier ||
|| **entity_data_1**
[`string`](../data-types.md) | External data for the chat ||
|| **entity_data_2**
[`string`](../data-types.md) | External data for the chat ||
|| **entity_data_3**
[`string`](../data-types.md) | External data for the chat ||
|| **date_create**
[`datetime`](../data-types.md) | Date of chat creation in ATOM format ||
|| **message_type**
[`string`](../data-types.md) | Type of chat messages ||
|| **mute_list**
[`array`](../data-types.md) | List of users who have disabled notifications ||
|| **manager_list**
[`array`](../data-types.md) | List of chat manager identifiers ||
|| **user_counter**
[`integer`](../data-types.md) | Number of chat participants ||
|| **restrictions**
[`object`](../data-types.md) | Restrictions on actions in the chat [(detailed description)](#chat-restrictions) ||
|| **role**
[`string`](../data-types.md) | Current user's role in the chat ||
|| **text_field_enabled**
[`boolean`](../data-types.md) | Availability of the message input field ||
|| **background_id**
[`integer`](../data-types.md) | Identifier of the chat background. If not set, the value is `null` ||
|| **entity_link**
[`object`](../data-types.md) | Link to the related object [(detailed description)](#chat-entity-link) ||
|| **permissions**
[`object`](../data-types.md) | Permissions for actions in the chat [(detailed description)](#chat-permissions) ||
|| **public**
[`string`](../data-types.md) | Indicator of the chat's public status ||
|#

#### Object restrictions {#chat-restrictions}

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
[`boolean`](../data-types.md) | Availability of notification disabling ||
|| **leave**
[`boolean`](../data-types.md) | Availability of leaving the chat ||
|| **leave_owner**
[`boolean`](../data-types.md) | Availability of the owner leaving the chat ||
|| **send**
[`boolean`](../data-types.md) | Availability of sending messages ||
|| **user_list**
[`boolean`](../data-types.md) | Availability of viewing the list of participants ||
|#

#### Object entity_link {#chat-entity-link}

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

#### Object permissions {#chat-permissions}

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

## Error Handling

{% include notitle [error handling](../../_includes/error-info.md) %}

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./im-recent-list.md)
- [{#T}](./im-dialog-get.md)
- [{#T}](./im-counters-get.md)