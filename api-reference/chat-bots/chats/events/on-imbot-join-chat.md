# Connecting the Chatbot to the Chat ONIMBOTJOINCHAT

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- parameter requirements are not indicated
- parameter tables are generated based on examples. What is [39] in the example??? BOT_ID? And how to record this in the table?

{% endnote %}

{% endif %}

> Scope: [`imbot`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

The event `ONIMBOTJOINCHAT` is for the chatbot to receive information about being added to a chat (or personal conversation).

{% note warning %}

The fields described below are the contents of the [data] field in the event. The authorization data in the [auth] key contains the data of the user who initiated the event; to obtain the bot's authorization data, you need to use [data][BOT][__BOT_CODE__].

{% endnote %}

#|
|| **Field** | **Description** | **Revision** ||
|| **BOT** 
[`unknown`](../../../data-types.md) | Array of chatbot codes intended for the event | ||
|| **PARAMS** 
[`unknown`](../../../data-types.md) | Array of event data | ||
|| **USER** 
[`unknown`](../../../data-types.md) | Array of user data, may be empty if ID = 0 | ||
|#

## BOT / [39]???

#|
|| **Field** | **Description** | **Revision** ||
|| **AUTH** 
[`unknown`](../../../data-types.md) | Parameters for authorization under the chatbot to perform actions | ||
|| **BOT_ID** 
[`unknown`](../../../data-types.md) | Identifier of the chatbot | ||
|| **BOT_CODE** 
[`unknown`](../../../data-types.md) | Code of the chatbot | ||
|#

## PARAMS

#|
|| **Field** | **Description** | **Revision** ||
|| **DIALOG_ID** 
[`unknown`](../../../data-types.md) | Identifier of the dialog | ||
|| **BOT_ID** 
[`unknown`](../../../data-types.md) | Identifier of the bot | ||
|| **CHAT_TYPE** 
[`unknown`](../../../data-types.md) | Type of message and chat
- **P** (one-on-one chat),
- **C** (limited number of participants),
- **O** (public chat),
- **S** (chatbot with elevated privileges - supervisor) | ||
|| **CHAT_ENTITY_TYPE** 
[`unknown`](../../../data-types.md) | Subtype of chat, can take the value LINES (Open channels) or be empty  | ||
|| **USER_ID** 
[`unknown`](../../../data-types.md) | Identifier of the user (for one-on-one chat - the one who opened the chatbot, for group chats - the one who invited the chatbot) | ||
|| **LANGUAGE** 
[`unknown`](../../../data-types.md) | Identifier of the default portal language | ||
|#

## USER

#|
|| **Field** | **Description** | **Revision** ||
|| **ID** 
[`unknown`](../../../data-types.md) | Identifier of the user | ||
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
        [BOT_ID] => 39
        [CHAT_TYPE] => P
        [CHAT_ENTITY_TYPE] => 'LINES'
        [USER_ID] => 1
        [LANGUAGE] => de
    )
    [USER] => Array (
        [ID] => 1
        [NAME] => Eugene Shelenkov
        [FIRST_NAME] => Eugene
        [LAST_NAME] => Shelenkov
        [WORK_POSITION] =>
        [GENDER] => M
    )
    ```

{% endlist %}

{% include [Footnote on examples](../../../../_includes/examples.md) %}