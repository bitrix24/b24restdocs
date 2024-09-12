# Change Chat Title imbot.chat.updateTitle

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- revisions needed for writing standards
- parameter types are not specified
- parameter requirements are not indicated
- examples are missing
- success response is absent
- error response is absent

{% endnote %}

{% endif %}

> Scope: [`imbot`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `imbot.chat.updateTitle` updates the chat title.

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **CHAT_ID**
[`unknown`](../../data-types.md) | `13` | Chat identifier | ||
|| **TITLE**
[`unknown`](../../data-types.md) | `'New name for the chat'` | New title | ||
|| **BOT_ID**
[`unknown`](../../data-types.md) | `39` | Identifier of the chatbot making the request. Can be omitted if there is only one chatbot | ||
|#

## Examples

{% include [Explanation about restCommand](../_includes/rest-command.md) %}

```php
$result = restCommand(
    'imbot.chat.updateTitle',
    Array(
        'CHAT_ID' => 13,
        'TITLE' => 'New name for the chat',
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
|| **TITLE_EMPTY** | New chat title was not provided. ||
|| **WRONG_REQUEST** | The title is already set or the specified chat does not exist. ||
|#