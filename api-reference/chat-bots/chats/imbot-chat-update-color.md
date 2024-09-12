# Change Chat Color imbot.chat.updateColor

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- revisions needed for writing standards
- parameter types not specified
- parameter requirements not indicated
- examples are missing
- success response is absent
- error response is absent

{% endnote %}

{% endif %}

> Scope: [`imbot`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `imbot.chat.updateColor` updates the chat color.

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **CHAT_ID**
[`unknown`](../../data-types.md) | `13` | Chat identifier | ||
|| **COLOR**
[`unknown`](../../data-types.md) | `'MINT'` | Chat color for the mobile application - RED, GREEN, MINT, LIGHT_BLUE, DARK_BLUE, PURPLE, AQUA, PINK, LIME, BROWN, AZURE, KHAKI, SAND, MARENGO, GRAY, GRAPHITE | ||
|| **BOT_ID**
[`unknown`](../../data-types.md) | `39` | Identifier of the chatbot making the request. Can be omitted if there is only one chatbot | ||
|#

## Examples

{% include [Explanation about restCommand](../_includes/rest-command.md) %}

```php
$result = restCommand(
    'imbot.chat.updateColor',
    Array(
        'CHAT_ID' => 13,
        'COLOR' => 'MINT',
        'BOT_ID' => 39,
    ),
    $_REQUEST[
        "auth"
    ]
);
```

{% include [Footnote about examples](../../../_includes/examples.md) %}

## Success Response

`true`

## Error Response

error

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| **CHAT_ID_EMPTY** | Chat identifier not provided. ||
|| **WRONG_COLOR** | Color is not in the list of available colors. ||
|| **WRONG_REQUEST** | Color is already set or the specified chat does not exist. ||
|#