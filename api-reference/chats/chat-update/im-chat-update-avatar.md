# Update Chat Avatar im.chat.updateAvatar

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- adjustments needed for writing standards
- parameter types are not specified
- examples are missing

{% endnote %}

{% endif %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.chat.updateAvatar` updates the chat avatar.

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **CHAT_ID^*^**
[`unknown`](../../data-types.md) | `13` | Chat identifier | 18 ||
|| **AVATAR^*^**
[`unknown`](../../data-types.md) | `/* base64 image */` | Image in base64 format | 18 ||
|#

{% include [Parameter Notes](../../../_includes/required.md) %}

## Examples

{% include [Explanation of restCommand](../_includes/rest-command.md) %}

```php
$result = restCommand(
    'im.chat.updateAvatar',
    Array(
        'CHAT_ID' => 13,
        'AVATAR' => '/* base64 image */'
    ),
    $_REQUEST[
        "auth"
    ]
);
```

{% include [Examples Notes](../../../_includes/examples.md) %}

## Response on Success

```json
{
    "result": true
}
```

## Response on Error

```json
{
    "error": "CHAT_ID_EMPTY",
    "error_description": "Chat identifier not provided"
}
```

### Description of Keys

- `error` – code of the occurred error
- `error_description` – brief description of the occurred error

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| **CHAT_ID_EMPTY** | Chat identifier not provided ||
|| **AVATAR_ERROR** | Incorrect image format provided ||
|| **WRONG_REQUEST** | Chat does not exist ||
|#