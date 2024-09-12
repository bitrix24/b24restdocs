# Get a directory item by ID crm.status.get

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter type is not specified
- examples (in other languages) are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user

```http
crm.status.get(id)
```

The method returns a directory item by its ID.

#|
|| **Parameter** | **Description** ||
|| **id^*^** | The ID of the directory item. ||
|#

{% include [Note on parameters](../../../_includes/required.md) %}

## Examples

```javascript
var id = prompt("Enter ID");
BX24.callMethod(
    "crm.status.get",
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

{% include [Note on examples](../../../_includes/examples.md) %}