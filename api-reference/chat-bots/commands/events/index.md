# Receiving the ONIMCOMMANDADD Event

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- parameter mandatory status is not indicated
- parameter tables are generated as examples. What is [14] in the example???

{% endnote %}

{% endif %}

> Scope: [`im`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

The `ONIMCOMMANDADD` event for application installation.

{% note warning %}

The fields described below are the contents of the [data] field in the event. The authorization data in the [auth] key contains the data of the user who initiated the event. To obtain the bot's authorization data, you need to use [data][BOT][__BOT_CODE__].

{% endnote %}

#|
|| **Field** | **Description** | **Revision** ||
|| **COMMAND** 
[`unknown`](../../../data-types.md) | Array of commands that were invoked by the user | ||
|| **PARAMS** 
[`unknown`](../../../data-types.md) | Array of message data | ||
|| **USER** 
[`unknown`](../../../data-types.md) | Array of the message author's data, may be empty if ID = 0 | ||
|#

## COMMAND / [14]???

#|
|| **Field** | **Description** | **Revision** ||
|| **AUTH** 
[`unknown`](../../../data-types.md) | Parameters for authorization under the chatbot to perform actions | ||
|| **BOT_ID** 
[`unknown`](../../../data-types.md) | Identifier of the chatbot | ||
|| **BOT_CODE** 
[`unknown`](../../../data-types.md) | Code of the chatbot | ||
|| **COMMAND** 
[`unknown`](../../../data-types.md) | Invoked command | ||
|| **COMMAND_ID** 
[`unknown`](../../../data-types.md) | Identifier of the command | ||
|| **COMMAND_PARAMS** 
[`unknown`](../../../data-types.md) | Parameters with which the command was invoked | ||
|| **COMMAND_CONTEXT** 
[`unknown`](../../../data-types.md) | Context of command execution.
- TEXTAREA, if the command was entered manually
- KEYBOARD, if a button was pressed on the keyboard | ||
|| **MESSAGE_ID** 
[`unknown`](../../../data-types.md) | Identifier of the message to which a response is needed | ||
|#

## PARAMS

#|
|| **Field** | **Description** | **Revision** ||
|| **DIALOG_ID** 
[`unknown`](../../../data-types.md) | Identifier of the dialog | ||
|| **CHAT_TYPE** 
[`unknown`](../../../data-types.md) | Type of message and chat
- P (one-on-one chat),
- C (limited number of participants),
- O (public chat) | ||
|| **MESSAGE_ID** 
[`unknown`](../../../data-types.md) | Identifier of the message | ||
|| **MESSAGE** 
[`unknown`](../../../data-types.md) | Message | ||
|| **MESSAGE_ORIGINAL** 
[`unknown`](../../../data-types.md) | Original message with bot BB-code (parameter available only in group chats) | ||
|| **FROM_USER_ID** 
[`unknown`](../../../data-types.md) | Identifier of the user who sent the message | ||
|| **TO_USER_ID** 
[`unknown`](../../../data-types.md) | Identifier of another user (parameter available only in one-on-one chats) | ||
|| **TO_CHAT_ID** 
[`unknown`](../../../data-types.md) | Identifier of the chat (parameter available only in group chats) | ||
|| **LANGUAGE** 
[`unknown`](../../../data-types.md) | Identifier of the default account language | ||
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

```js
[COMMAND] => Array (
    [14] => Array (
        [AUTH] => Array (
            [domain] => b24.hazz
            [member_id] => d41d8cd98f00b204e9800998ecf8427e
            [application_token] => 8006ddd764e69deb28af0c768b10ed65
        )
        [BOT_ID] => 62
        [BOT_CODE] => echobot
        [COMMAND] => echo
        [COMMAND_ID] => 14
        [COMMAND_PARAMS] => test
        [COMMAND_CONTEXT] => TEXTAREA
        [MESSAGE_ID] => 1221
    )
)
[PARAMS] => Array (
    [DIALOG_ID] => 1
    [CHAT_TYPE] => P
    [MESSAGE_ID] => 1221
    [MESSAGE] => /echo test
    [MESSAGE_ORIGINAL] => /echo test
    [FROM_USER_ID] => 1
    [TO_USER_ID] => 2
    [TO_CHAT_ID] => 6
    [LANGUAGE] => en
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

{% include [Footnote on examples](../../../../_includes/examples.md) %}