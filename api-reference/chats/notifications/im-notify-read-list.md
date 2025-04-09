# Read the list of notifications (excluding CONFIRM) im.notify.read.list

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- examples are missing

{% endnote %}

{% endif %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.notify.read.list` "reads" the list of notifications, excluding notifications of type CONFIRM.

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **IDS^*^**
[`unknown`](../../data-types.md) | `[1,2,3]` | Array of notification identifiers | 30 ||
|| **ACTION**
[`unknown`](../../data-types.md) | `'Y'` | Mark as read/unread (`Y`\|`N`) | 30 ||
|#

{% include [Footnote about parameters](../../../_includes/required.md) %}

## Examples

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'im.notify.read.list',
        {
            IDS: [1,2,3],
            ACTION: 'Y'
        },
        res => {
            if (res.error())
            {
            console.error(result.error().ex);
            }
            else
            {
            console.log(res.data())
            }
        }
    );
    ```

{% endlist %}

{% include [Footnote about examples](../../../_includes/examples.md) %}

## Response on success

```json
{
    "result": true
}        
```

## Response on error

```json
{
    "error": "PARAMS_ERROR",
    "error_description": "No IDS param or it is not an array"
}
```

### Description of keys

- `error` – code of the occurred error
- `error_description` – brief description of the occurred error

### Possible error codes

#|
|| **Code** | **Description** ||
|| **PARAMS_ERROR** | The `IDS` parameter was not provided or it is not an array ||
|#