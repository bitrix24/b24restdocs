# Change Chat Color im.chat.updateColor

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- revisions needed for writing standards
- parameter types are not specified
- examples are missing

{% endnote %}

{% endif %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.chat.updateColor` updates the chat color.

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **CHAT_ID^*^**
[`unknown`](../../data-types.md) | `13` | Chat identifier | 18 ||
|| **COLOR^*^**
[`unknown`](../../data-types.md) | `MINT` | Chat color for the mobile application (RED, GREEN, MINT, LIGHT_BLUE, DARK_BLUE, PURPLE, AQUA, PINK, LIME, BROWN, AZURE, KHAKI, SAND, MARENGO, GRAY, GRAPHITE) | 18 ||
|#

{% include [Parameter Note](../../../_includes/required.md) %}

## Examples

{% include [Explanation of restCommand](../_includes/rest-command.md) %}

```php
$result = restCommand(
    'im.chat.updateColor',
    Array(
        'CHAT_ID' => 13,
        'COLOR' => 'MINT'
    ),
    $_REQUEST[
        "auth"
    ]
);
```

{% include [Examples Note](../../../_includes/examples.md) %}

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
|| **WRONG_COLOR** | Color is not in the list of available colors ||
|| **WRONG_REQUEST** | Color is already set or the specified chat does not exist ||
|#