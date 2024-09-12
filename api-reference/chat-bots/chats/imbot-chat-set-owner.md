# Change Chat Owner on Behalf of the Chat Bot imbot.chat.setOwner

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed to meet writing standards
- parameter types not specified
- parameter requirements not indicated
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`imbot`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `imbot.chat.setOwner` changes the owner of the chat on behalf of the chat bot.

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **CHAT_ID**
[`unknown`](../../data-types.md) | `13` | Identifier of the chat | ||
|| **USER_ID**
[`unknown`](../../data-types.md) | `'2'` | Identifier of the new owner | ||
|| **BOT_ID**
[`unknown`](../../data-types.md) | `39` | Identifier of the bot making the request. It can be omitted if there is only one. | ||
|#

## Examples

{% include [Explanation about restCommand](../_includes/rest-command.md) %}

```php
$result = restCommand(
    'imbot.chat.setOwner',
    Array(
        'CHAT_ID' => 13,
        'USER_ID' => '2',
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

## Possible Error Codes

#|
|| **Code** | **Description** ||
|| **CHAT_ID_EMPTY** | Chat identifier not provided. ||
|| **USER_ID_EMPTY** | New owner's identifier not provided. ||
|| **WRONG_REQUEST** | Method called not by the chat owner or the specified user is not a participant in the chat. ||
|#