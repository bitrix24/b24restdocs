# Delete Notifications im.notify.delete

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- adjustments needed for writing standards
- parameter types are not specified
- examples are missing
- links to pages that have not yet been created are not provided

{% endnote %}

{% endif %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.notify.delete` removes a notification.

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **ID^*^**
[`unknown`](../../data-types.md) | `123` | Notification identifier | 18 ||
|| **TAG^*^**
[`unknown`](../../data-types.md) | `TEST` | Notification tag, unique within the system. | 18 ||
|| **SUB_TAG^*^**
[`unknown`](../../data-types.md) | `SUB`\|`TEST` | Additional tag, without uniqueness check | 18 ||
|#

{% include [Note on parameters](../../../_includes/required.md) %}

{% note warning %}

You must specify **one of the three** required parameters: `ID` (notification identifier), `TAG` (notification tag), or `SUB_TAG` (additional tag).

{% endnote %}

## Examples

{% include [Explanation about restCommand](../_includes/rest-command.md) %}

```php
$result = restCommand(
    'im.notify.delete',
    Array(
        'ID' => 13,
        'TAG' => 'TEST',
        'SUB_TAG' => 'SUB|TEST'
    ),
    $_REQUEST[
        "auth"
    ]
);
```

{% include [Note on examples](../../../_includes/examples.md) %}

## Response on Success

```json
{
    "result": true
}
```

**Execution result**: `true` or an error.

## Response on Error

```json
{
    "error": "PARAMS_ERROR",
    "error_description": "Error deleting notification"
}
```

### Description of Keys:

- `error` – code of the occurred error
- `error_description` – brief description of the occurred error

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| **PARAMS_ERROR** | Error deleting notification ||
|#

## Related Links

- [{#T}](../messages/attachments/index.md)