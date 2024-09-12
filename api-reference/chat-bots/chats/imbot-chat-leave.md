# Chat-bot Exit from Specified Chat imbot.chat.leave

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- parameter requirements are not indicated
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`imbot`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `imbot.chat.leave` allows the chat-bot to exit from the specified chat.

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **CHAT_ID**
[`unknown`](../../data-types.md) | `13` | Identifier of the chat to exit from | ||
|| **BOT_ID**
[`unknown`](../../data-types.md) | `39` | Identifier of the chat-bot making the request. It can be omitted if there is only one chat-bot | ||
|#

## Examples

{% include [Explanation about restCommand](../_includes/rest-command.md) %}

```php
$result = restCommand(
    'imbot.chat.leave',
    Array(
        'CHAT_ID' => 13,
        'BOT_ID' => 39,
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
|| **CHAT_ID_EMPTY** | Chat identifier was not provided. ||
|| **ACCESS_ERROR** | You do not have sufficient permissions to send the status in this chat. ||
|| **BOT_ID_ERROR** | Chat-bot not found. ||
|#