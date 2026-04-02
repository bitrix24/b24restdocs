# Chat Fields

Chat data that can be retrieved using the method [imbot.dialog.get](./imbot-dialog-get.md).

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Chat identifier ||
|| **parent_chat_id**
[`integer`](../../../data-types.md) | Parent chat identifier ||
|| **parent_message_id**
[`integer`](../../../data-types.md) | Parent message identifier ||
|| **name**
[`string`](../../../data-types.md) | Chat name ||
|| **description**
[`string`](../../../data-types.md) | Chat description ||
|| **owner**
[`integer`](../../../data-types.md) | Chat owner identifier ||
|| **extranet**
[`boolean`](../../../data-types.md) | Indicates if it is an extranet chat ||
|| **avatar**
[`string`](../../../data-types.md) | URL of the chat avatar ||
|| **color**
[`string`](../../../data-types.md) | Chat color in HEX format ||
|| **type**
[`string`](../../../data-types.md) | Chat type ||
|| **counter**
[`integer`](../../../data-types.md) | Message counter ||
|| **user_counter**
[`integer`](../../../data-types.md) | User counter ||
|| **message_count**
[`integer`](../../../data-types.md) | Number of messages in the chat ||
|| **unread_id**
[`integer`](../../../data-types.md) | Identifier of the first unread message ||
|| **restrictions**
[`object`](../../../data-types.md) | Object with [description of restrictions](#restrictions) ||
|| **last_message_id**
[`integer`](../../../data-types.md) | Identifier of the last message ||
|| **last_id**
[`integer`](../../../data-types.md) | Identifier of the last message ||
|| **marked_id**
[`integer`](../../../data-types.md) | Identifier of the marked message ||
|| **disk_folder_id**
[`integer`](../../../data-types.md) | Identifier of the folder on Drive ||
|| **entity_type**
[`string`](../../../data-types.md) | Type of the related object ||
|| **entity_id**
[`string`](../../../data-types.md) | Identifier of the related object ||
|| **entity_data_1**
[`string`](../../../data-types.md) | Additional data of the object ||
|| **entity_data_2**
[`string`](../../../data-types.md) | Additional data of the object ||
|| **entity_data_3**
[`string`](../../../data-types.md) | Additional data of the object ||
|| **mute_list**
[`array`](../../../data-types.md) | List of users with notifications turned off ||
|| **date_create**
[`string`](../../../data-types.md) | Creation date in `ISO 8601` format ||
|| **message_type**
[`string`](../../../data-types.md) | Type of chat messages ||
|| **public**
[`string`](../../../data-types.md) | Public identifier of the chat ||
|| **role**
[`string`](../../../data-types.md) | Role of the current user in the chat ||
|| **entity_link**
[`object`](../../../data-types.md) | Object with [link to the entity](#entity-link) ||
|| **text_field_enabled**
[`boolean`](../../../data-types.md) | Is text input allowed ||
|| **background_id**
[`integer`](../../../data-types.md) | Identifier of the chat background ||
|| **permissions**
[`object`](../../../data-types.md) | Object with [chat permissions](#permissions) ||
|| **is_new**
[`boolean`](../../../data-types.md) | Indicates if it is a new chat ||
|| **readed_list**
[`array`](../../../data-types.md) | List of [users who read the message](#readed-list-item) ||
|| **manager_list**
[`array`](../../../data-types.md) | List of chat manager identifiers ||
|| **last_message_views**
[`object`](../../../data-types.md) | Information about [views of the last message](#last-message-views) ||
|| **dialog_id**
[`string`](../../../data-types.md) | Dialog identifier ||
|#

## Restrictions Object {#restrictions}

#|
|| **Name**
`type` | **Description** ||
|| **avatar**
[`boolean`](../../../data-types.md) | Can change the avatar ||
|| **rename**
[`boolean`](../../../data-types.md) | Can rename the chat ||
|| **extend**
[`boolean`](../../../data-types.md) | Can add participants ||
|| **call**
[`boolean`](../../../data-types.md) | Can make calls ||
|| **mute**
[`boolean`](../../../data-types.md) | Can turn off notifications ||
|| **leave**
[`boolean`](../../../data-types.md) | Can leave the chat ||
|| **leave_owner**
[`boolean`](../../../data-types.md) | Can the owner leave the chat ||
|| **send**
[`boolean`](../../../data-types.md) | Can send messages ||
|| **user_list**
[`boolean`](../../../data-types.md) | Can get the list of participants ||
|#

## Entity Link Object {#entity-link}

#|
|| **Name**
`type` | **Description** ||
|| **type**
[`string`](../../../data-types.md) | Type of the related object ||
|| **url**
[`string`](../../../data-types.md) | Link to the related object ||
|| **id**
[`string`](../../../data-types.md) | Identifier of the related object ||
|#

## Permissions Object {#permissions}

#|
|| **Name**
`type` | **Description** ||
|| **manage_users_add**
[`string`](../../../data-types.md) | Minimum role for adding users ||
|| **manage_users_delete**
[`string`](../../../data-types.md) | Minimum role for deleting users ||
|| **manage_ui**
[`string`](../../../data-types.md) | Minimum role for managing the interface ||
|| **manage_settings**
[`string`](../../../data-types.md) | Minimum role for managing settings ||
|| **manage_messages**
[`string`](../../../data-types.md) | Minimum role for managing messages ||
|| **can_post**
[`string`](../../../data-types.md) | Minimum role for sending messages ||
|#

## Readed List Item Object {#readed-list-item}

#|
|| **Name**
`type` | **Description** ||
|| **user_id**
[`integer`](../../../data-types.md) | User identifier ||
|| **user_name**
[`string`](../../../data-types.md) | User name ||
|| **message_id**
[`integer`](../../../data-types.md) | Identifier of the last read message ||
|| **date**
[`string`](../../../data-types.md) | Read date in `ISO 8601` format ||
|#

## Last Message Views Object {#last-message-views}

#|
|| **Name**
`type` | **Description** ||
|| **message_id**
[`integer`](../../../data-types.md) | Message identifier ||
|| **first_viewers**
[`array`](../../../data-types.md) | List of [first viewers](#last-message-views-first-viewers-item) ||
|| **count_of_viewers**
[`integer`](../../../data-types.md) | Number of views ||
|#

## Last Message Views First Viewers Item Object {#last-message-views-first-viewers-item}

#|
|| **Name**
`type` | **Description** ||
|| **user_id**
[`integer`](../../../data-types.md) | User identifier ||
|| **user_name**
[`string`](../../../data-types.md) | User name ||
|| **date**
[`string`](../../../data-types.md) | View date in `ISO 8601` format ||
|#