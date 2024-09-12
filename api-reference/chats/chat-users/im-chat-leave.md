# Leave Chat im.chat.leave

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- adjustments needed for writing standards
- parameter types not specified
- examples are missing

{% endnote %}

{% endif %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The `im.chat.leave` method is used to leave a chat.

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **CHAT_ID^*^**
[`unknown`](../../data-types.md) | `13` | Chat identifier | 18 ||
|#

{% include [Parameter Note](../../../_includes/required.md) %}

## Examples

{% include [Explanation of restCommand](../_includes/rest-command.md) %}

```php
$result = restCommand(
    'im.chat.leave',
    Array(
        'CHAT_ID' => 13
    ),
    $_REQUEST[
        "auth"
    ]
);
```

{% include [Example Note](../../../_includes/examples.md) %}

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
|| **WRONG_REQUEST** | You are not a member of this chat ||
|#