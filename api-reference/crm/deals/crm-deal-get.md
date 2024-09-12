# Get deal by Id crm.deal.get

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

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

The method `crm.deal.get` returns a deal by its identifier.

#|
|| **Parameter** | **Description** ||
|| **id**^*^ | Identifier of the deal. ||
|#

{% include [Note on parameters](../../../_includes/required.md) %}

## Example

```js
var id = prompt("Enter ID");
BX24.callMethod(
    "crm.deal.get",
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


{% note tip "Related methods and topics" %}

[{#T}](./recurring-deals/crm-deal-recurring-get.md)

{% endnote %}