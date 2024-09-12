# Delete the set of contacts associated with the specified deal crm.deal.contact.items.delete

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon

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

The method `crm.deal.contact.items.delete` clears the set of contacts associated with the specified deal.

#|
|| **Parameter** | **Description** ||
|| **id**^*^ | Identifier of the deal. ||
|#

{% include [Parameter notes](../../../../_includes/required.md) %}

## Example

```js
var id = prompt("Enter ID");
BX24.callMethod(
    "crm.deal.contact.items.delete",
    {
        id: id
    },
    function(result)
    {
        if(result.error())
            console.error(result.error());
        else
            console.info(result.data());
    }
);
```

{% include [Parameter notes](../../../../_includes/required.md) %}