# Get the list of participants imbot.chat.user.list

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- revisions needed for writing standards
- parameter types not specified
- parameter requirements not indicated
- examples missing
- success response missing
- error response missing

{% endnote %}

{% endif %}

> Scope: [`imbot`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `imbot.chat.user.list` retrieves the list of participants.

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **CHAT_ID**
[`unknown`](../../data-types.md) | `13` | Identifier of the chat | ||
|| **BOT_ID**
[`unknown`](../../data-types.md) | `39` | Identifier of the chatbot making the request. It can be omitted if there is only one chatbot | ||
|#

## Examples

{% include [Explanation about restCommand](../_includes/rest-command.md) %}

```php
$result = restCommand(
    'imbot.chat.user.list',
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

An array of participant identifiers.

## Error Response

error

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| **CHAT_ID_EMPTY** | Chat identifier not provided. ||
|#