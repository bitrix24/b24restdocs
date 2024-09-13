# Delete Notifications im.notify.delete

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- adjustments needed for writing standards
- parameter types not specified
- examples missing
- links to pages not yet created are not provided

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

{% include [Parameter Note](../../../_includes/required.md) %}

{% note warning %}

You must specify **one of the three** required parameters: `ID` (notification identifier), `TAG` (notification tag), or `SUB_TAG` (additional tag).

{% endnote %}

## Examples

{% include [Explanation of restCommand](../_includes/rest-command.md) %}

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

{% include [Examples Note](../../../_includes/examples.md) %}

## Successful Response

```json
{
    "result": true
}
```

**Execution Result**: `true` or an error.

## Error Response

```json
{
    "error": "PARAMS_ERROR",
    "error_description": "Error deleting notification"
}
```

### Key Descriptions:

- `error` – code of the occurred error
- `error_description` – brief description of the occurred error

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| **PARAMS_ERROR** | Error deleting notification ||
|#

## Related Links

- [{#T}](../messages/attachments/index.md)