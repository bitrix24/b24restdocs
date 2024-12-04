# Send the "Chat-bot is typing a message..." imbot.chat.sendTyping

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- parameter requirements are not indicated
- examples are missing
- success response is absent
- error response is absent
- links to pages that have not yet been created are not provided

{% endnote %}

{% endif %}

> Scope: [`imbot`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `imbot.chat.sendTyping` sends the message "Chat-bot is typing a message...".

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **BOT_ID**
[`unknown`](../../data-types.md) | `39` | Identifier of the chat-bot from which the request is made; can be omitted if there is only one bot | ||
|| **DIALOG_ID**
[`unknown`](../../data-types.md) | `1` | Identifier of the dialog, which is either the USER_ID of the user or chatXX - where XX is the chat identifier, passed in the ONIMBOTMESSAGEADD and ONIMJOINCHAT events | ||
|#

## Examples

{% include [Explanation about restCommand](../_includes/rest-command.md) %}

{% list tabs %}

- PHP

    ```php
    $result = restCommand(
        'imbot.chat.sendTyping',
        Array(
            'BOT_ID' => 39,
            'DIALOG_ID' => 1,
        ),
        $_REQUEST[
            "auth"
        ]
    );
    ```

{% endlist %}

{% include [Footnote about examples](../../../_includes/examples.md) %}

## Success response

`true`.

## Error response

error

### Possible error codes

#|
|| **Code** | **Description** ||
|| **BOT_ID_ERROR** | Chat-bot not found. ||
|| **DIALOG_ID_EMPTY** | Dialog identifier not provided. ||
|#

## Related links

- [Event for receiving a message by the chat-bot ONIMBOTMESSAGEADD](./events/index.md)
- [Event for receiving information by the chat-bot about being added to a chat (or personal conversation) ONIMJOINCHAT](../chats/events/on-imbot-join-chat.md)