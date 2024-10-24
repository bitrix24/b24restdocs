# Switch the dialog to a free operator imopenlines.bot.session.operator

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

This method switches the conversation to a free operator.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#| 
|| **Name** 
`Type` | **Example** | **Description** | **Revision** ||
|| **CHAT_ID^*^** 
[`integer`](../../../data-types.md) | `12` | Identifier of the chat | 1 ||
|#

## Examples

{% include [Explanation of restCommand](../../../chat-bots/_includes/rest-command.md) %}

```php
$result = restCommand(
    'imopenlines.bot.session.operator',
    Array(
        'CHAT_ID' => 12
    ),
    $_REQUEST["auth"]
);
```

{% include [Note on examples](../../../../_includes/examples.md) %}

## Response on success

```json
{
    "result": true
}
```

## Response on error

```json
{
    "error": "CHAT_ID_EMPTY",
    "error_description": "Chat identifier not provided"
}
```

### Description of keys

- `error` – code of the occurred error
- `error_description` – brief description of the occurred error

### Possible error codes

#| 
|| **Code** | **Description** ||
|| **CHAT_ID_EMPTY** | Chat identifier not provided ||
|| **WRONG_CHAT** | The specified chat is not managed by the bot ||
|| **BOT_ID_ERROR** | Incorrect chat-bot identifier ||
|#