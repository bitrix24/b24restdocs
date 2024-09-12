# Get Chat Participant IDs im.chat.user.list

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

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

The method `im.chat.user.list` retrieves a list of chat participants.

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **CHAT_ID^*^**
[`unknown`](../../data-types.md) | `13` | Chat identifier | 18 ||
|#

{% include [Parameter Notes](../../../_includes/required.md) %}

## Examples

{% include [Explanation of restCommand](../_includes/rest-command.md) %}

```php
$result = restCommand(
    'im.chat.user.list',
    Array(
        'CHAT_ID' => 13
    ),
    $_REQUEST[
        "auth"
    ]
);
```

{% include [Example Notes](../../../_includes/examples.md) %}

## Successful Response

```json
{
    "result": [1,2]
}
```

## Error Response

```json
{
    "error": "CHAT_ID_EMPTY",
    "error_description": "Chat identifier not provided"
}
```

### Key Descriptions:

- `error` – code of the occurred error
- `error_description` – brief description of the occurred error

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| **CHAT_ID_EMPTY** | Chat identifier not provided, or an invalid identifier was given. ||
|#