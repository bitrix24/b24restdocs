# Get Chat ID im.chat.get

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- edits needed for writing standards
- parameter types not specified
- examples missing
- error response not provided
- links to yet-to-be-created pages not specified
- from Sergey's file: unclear in what context the method is applicable, needs clarification

{% endnote %}

{% endif %}

> Scope: [`im`](../scopes/permissions.md)
>
> Who can execute the method: any user

The `im.chat.get` method retrieves the chat ID.

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **ENTITY_TYPE^*^**
[`unknown`](../data-types.md) | `CRM` | Entity identifier. Can be used to find the chat and to easily determine the context in event handlers:
- [ONIMBOTMESSAGEADD](.),
- [ONIMBOTMESSAGEUPDATE](.),
- [ONIMBOTMESSAGEDELETE](.) | 18 ||
|| **ENTITY_ID^*^**
[`unknown`](../data-types.md) | `LEAD`\|`13` | Numeric entity identifier. Can be used to find the chat and to easily determine the context in event handlers:
- [ONIMBOTMESSAGEADD](.),
- [ONIMBOTMESSAGEUPDATE](.),
- [ONIMBOTMESSAGEDELETE](.) | 18 ||
|#

{% include [Parameter Notes](../../_includes/required.md) %}

## Examples

{% include [Explanation of restCommand](./_includes/rest-command.md) %}

```php
$result = restCommand(
    'im.chat.get',
    Array(
        'ENTITY_TYPE' => 'CRM',
        'ENTITY_ID' => 'LEAD|13',
    ),
    $_REQUEST[
        "auth"
    ]
);
```

{% include [Example Notes](../../_includes/examples.md) %}

## Successful Response

```json
{
    "result": 10
}
```

**Execution Result**: returns the chat ID `CHAT_ID` or `null`.