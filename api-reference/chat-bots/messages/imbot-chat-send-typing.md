# Send Typing Indicator "Chat-bot is typing a message..." imbot.chat.sendTyping

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- edits needed for writing standards
- parameter types not specified
- parameter requirements not indicated
- examples missing
- success response not provided
- error response not provided
- links to yet-to-be-created pages not specified

{% endnote %}

{% endif %}

> Scope: [`imbot`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `imbot.chat.sendTyping` sends the message "Chat-bot is typing a message...".

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **BOT_ID**
[`unknown`](../../data-types.md) | `39` | Identifier of the chat-bot making the request; can be omitted if there is only one bot | ||
|| **DIALOG_ID**
[`unknown`](../../data-types.md) | `1` | Identifier of the dialog, which is either the USER_ID of the user or chatXX - where XX is the chat identifier, passed in the ONIMBOTMESSAGEADD and ONIMJOINCHAT events | ||
|#

## Examples

{% include [Explanation about restCommand](../_includes/rest-command.md) %}

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

{% include [Footnote about examples](../../../_includes/examples.md) %}

## Success Response

`true`.

## Error Response

error

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| **BOT_ID_ERROR** | Chat-bot not found. ||
|| **DIALOG_ID_EMPTY** | Dialog identifier not provided. ||
|#

## Related Links

- [Event for receiving a message by the chat-bot ONIMBOTMESSAGEADD](./events/index.md)
- [Event for receiving information by the chat-bot about being added to a chat (or personal conversation) ONIMJOINCHAT](../chats/events/on-imbot-join-chat.md)