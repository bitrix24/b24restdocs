# ONIMBOTMESSAGEDELETE Event

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- parameter requirements are not indicated
- parameter tables are generated based on examples. What is [39] in the example??? BOT_ID? And how should this be recorded in the table?

{% endnote %}

{% endif %}

> Scope: [`imbot`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

The `ONIMBOTMESSAGEDELETE` event occurs when a message is deleted.

{% note warning %}

The fields described below are the contents of the [data] field in the event. The authorization data in the [auth] key contains the data of the user who initiated the event; to obtain the bot's authorization data, you need to use [data][BOT][__BOT_CODE__].

{% endnote %}

#|
|| **Field** | **Description** | **Revision** ||
|| **BOT** 
[`unknown`](../../../data-types.md) | Array of chat-bot codes to which the message is intended | ||
|| **PARAMS** 
[`unknown`](../../../data-types.md) | Array of message data | ||
|| **USER** 
[`unknown`](../../../data-types.md) | Array of message author data, may be empty if ID = 0 | ||
|#

## BOT/[39]???

#|
|| **Field** | **Description** | **Revision** ||
|| **AUTH** 
[`unknown`](../../../data-types.md) | Parameters for authorization under the bot to perform actions | ||
|| **BOT_ID** 
[`unknown`](../../../data-types.md) | Bot identifier | ||
|| **BOT_CODE** 
[`unknown`](../../../data-types.md) | Bot code | ||
|#

## AUTH

#|
|| **Field** | **Description** | **Revision** ||
|| **domain** 
[`unknown`](../../../data-types.md) | Domain | ||
|| **member_id** 
[`unknown`](../../../data-types.md) | Unique identifier | ||
|| **application_token** 
[`unknown`](../../../data-types.md) | Application token | ||
|#

## PARAMS

#|
|| **Field** | **Description** | **Revision** ||
|| **DIALOG_ID** 
[`unknown`](../../../data-types.md) | Dialog identifier | ||
|| **CHAT_TYPE** 
[`unknown`](../../../data-types.md) | Type of message and chat, can be P (one-on-one chat), C (limited number of participants), O (public chat), S (chat-bot with elevated privileges - supervisor) | ||
|| **CHAT_ENTITY_TYPE** 
[`unknown`](../../../data-types.md) | for chat identification (this data field can be set at the time of creation), for open line chats this field will indicate LINES | ||
|| **CHAT_ENTITY_ID** 
[`unknown`](../../../data-types.md) | for chat identification (this data field can be set at the time of creation) | ||
|| **MESSAGE_ID** 
[`unknown`](../../../data-types.md) | Message identifier | ||
|| **MESSAGE** 
[`unknown`](../../../data-types.md) | Message | ||
|| **FROM_USER_ID** 
[`unknown`](../../../data-types.md) | Identifier of the user who sent the message | ||
|| **TO_CHAT_ID** 
[`unknown`](../../../data-types.md) | Identifier of the chat (parameter available only in group chats) | ||
|| **LANGUAGE** 
[`unknown`](../../../data-types.md) | Default portal language identifier | ||
|#

## USER

#|
|| **Field** | **Description** | **Revision** ||
|| **ID** 
[`unknown`](../../../data-types.md) | User identifier | ||
|| **NAME** 
[`unknown`](../../../data-types.md) | User's full name | ||
|| **FIRST_NAME** 
[`unknown`](../../../data-types.md) | User's first name | ||
|| **LAST_NAME** 
[`unknown`](../../../data-types.md) | User's last name | ||
|| **WORK_POSITION** 
[`unknown`](../../../data-types.md) | Job title | ||
|| **GENDER** 
[`unknown`](../../../data-types.md) | Gender, can be either M (male) or F (female) | ||
|| **IS_BOT** 
[`unknown`](../../../data-types.md) | Is this user a bot (Y), otherwise - N. | ||
|| **IS_CONNECTOR** 
[`unknown`](../../../data-types.md) | Is this user a connector (participant of the open line chat, client), otherwise - N. | ||
|| **IS_NETWORK** 
[`unknown`](../../../data-types.md) | Is this user a network user (can be a participant of the open line chat, client, or just an external user), otherwise - N. | ||
|| **IS_EXTRANET** 
[`unknown`](../../../data-types.md) | Is this user an extranet user (all users who are not intranet), otherwise - N. | ||
|#

## Examples

{% list tabs %}

- JS

    ```js
    [BOT] => Array (
        [39] => Array (
            [AUTH] => Array (
                [domain] => b24.hazz
                [member_id] => d41d8cd98f00b204e9800998ecf8427e
                [application_token] => 8006ddd764e69deb28af0c768b10ed65
            )
            [BOT_ID] => 39    
            [BOT_CODE] => newbot
        )
    )
    [PARAMS] => Array (
        [DIALOG_ID] => 1    
        [CHAT_TYPE] => P    
        [CHAT_ENTITY_TYPE] => 'LINES'    
        [CHAT_ENTITY_ID] => 13    
        [MESSAGE_ID] => 392
        [MESSAGE] => test3
        [FROM_USER_ID] => 1
        [TO_CHAT_ID] => 6
        [LANGUAGE] => de    
    )
    [USER] => Array (
        [ID] => 1
        [NAME] => John Smith
        [FIRST_NAME] => John
        [LAST_NAME] => Smith
        [WORK_POSITION] =>
        [GENDER] => M
        [IS_BOT] => 'Y'
        [IS_CONNECTOR] => 'Y'
        [IS_NETWORK] => 'Y'
        [IS_EXTRANET] => 'Y'
    )
    ```

{% endlist %}

{% include [Footnote on examples](../../../../_includes/examples.md) %}