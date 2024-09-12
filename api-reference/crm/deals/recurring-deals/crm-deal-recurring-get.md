# Get the fields of the recurring deal template settings by Id crm.deal.recurring.get

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter type is not specified
- examples are missing (in other languages)
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.deal.recurring.get` returns the fields of the recurring deal template settings by identifier.

#|
|| **Parameter** | **Description** ||
|| **id**^*^ | Identifier of the recurring deal template settings. ||
|#

{% include [Parameter notes](../../../../_includes/required.md) %}

## Example

```js
var id = prompt("Enter ID");
BX24.callMethod(
    "crm.deal.recurring.get",
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

{% include [Example notes](../../../../_includes/examples.md) %}