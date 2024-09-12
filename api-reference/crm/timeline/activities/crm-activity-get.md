# Get deal by id crm.activity.get

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- examples are missing
- success response is absent
- error response is absent
- from Sergey's file: mention that a special method should be used for configurable deals

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.activity.get` returns the activity by its identifier.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **id**
[`unknown`](../../../data-types.md) | Identifier of the activity. ||
|#

## Examples

```js
var id = prompt("Enter ID");
BX24.callMethod(
    "crm.activity.get",
    { id: id },
    function(result)
    {
        if(result.error())
            console.error(result.error());
        else
            console.dir(result.data());
    }
);
```

{% include [Note on examples](../../../../_includes/examples.md) %}