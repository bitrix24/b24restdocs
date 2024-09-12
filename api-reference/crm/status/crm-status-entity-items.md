# Get Directory Item by Its Symbolic Identifier crm.status.entity.items

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

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

The method returns directory items by its symbolic identifier, sorted by the "SORT" field. This method is similar to [crm.status.list](crm-status-list.md), except that the latter allows you to define sorting rules.

#|
|| **Parameter** | **Description** ||
|| **entityId^*^** | Symbolic identifier of the directory (can be obtained by calling the method [crm.status.entity.types](crm-status-entity-types.md)). ||
|#

{% include [Parameter Note](../../../_includes/required.md) %}

## Examples

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

{% include [Examples Note](../../../_includes/examples.md) %}