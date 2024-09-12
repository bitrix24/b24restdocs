# Update Chat Title im.chat.add

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- adjustments needed for writing standards
- parameter types not specified
- examples missing

{% endnote %}

{% endif %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.chat.add` updates the chat title.

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **CHAT_ID**
[`unknown`](../../data-types.md) | `13` | Chat identifier | 18 ||
|| **TITLE**
[`unknown`](../../data-types.md) | `New name for the chat` | New title | 18 ||
|#

## Examples

{% include [Explanation about restCommand](../_includes/rest-command.md) %}

```php
$result = restCommand(
    'im.chat.updateTitle',
    Array(
        'CHAT_ID' => 13,
        'TITLE' => 'New name for the chat'
    ),
    $_REQUEST[
        "auth"
    ]
);
```

{% include [Footnote about examples](../../../_includes/examples.md) %}

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

- `error` – code of the occurred error
- `error_description` – brief description of the occurred error

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| **CHAT_ID_EMPTY** | Chat identifier not provided ||
|| **TITLE_EMPTY** | New chat title not provided ||
|| **WRONG_REQUEST** | Title already set or specified chat does not exist ||
|#