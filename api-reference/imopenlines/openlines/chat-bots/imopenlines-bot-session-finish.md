# Finish the dialog imopenlines.bot.session.finish

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- examples are missing

{% endnote %}

{% endif %}

> Scope: [`imopenlines, imbot`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method ends the current session.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Example** | **Description** | **Revision** ||
|| **CHAT_ID^*^**
[`integer`](../../../data-types.md) | `112` | Chat identifier | 1 ||
|#

## Examples

{% include [Explanation about restCommand](../../../chat-bots/_includes/rest-command.md) %}

```php
$result = restCommand(
    'imopenlines.bot.session.finish',
    Array(
        'CHAT_ID' => 112
    ),
    $_REQUEST["auth"]
);
```

{% include [Note on examples](../../../../_includes/examples.md) %}

## Success Response

```json
{
    "result": true
}
```

## Error Response

```json
{
    "error": "CHAT_ID_EMPTY",
    "error_description": "Chat identifier not provided"
}
```

### Key Descriptions

- `error` — code of the occurred error
- `error_description` — brief description of the occurred error

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| **CHAT_ID_EMPTY** | Chat identifier not provided ||
|| **BOT_ID_ERROR** | Incorrect chat-bot identifier ||
|#