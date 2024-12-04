# Get the directory item by its symbolic identifier crm.status.entity.items

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter type is not specified
- examples are missing (in other languages)
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user

```http
crm.status.entity.items()
```

The method returns directory items by its symbolic identifier, sorted by the "SORT" field. This method is similar to [crm.status.list](crm-status-list.md), except that in the latter, sorting rules can be defined.

#|
|| **Parameter** | **Description** ||
|| **entityId^*^** | Symbolic identifier of the directory (can be obtained by calling the method [crm.status.entity.types](crm-status-entity-types.md)). ||
|#

{% include [Notes on parameters](../../../_includes/required.md) %}

## Examples

{% list tabs %}

- JS

    ```javascript
    var id = prompt('Enter ID');
    BX24.callMethod(
        "crm.status.entity.items",
        {
            entityId: id
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

{% endlist %}

{% include [Notes on examples](../../../_includes/examples.md) %}