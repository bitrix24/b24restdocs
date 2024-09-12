# Get IDs of Objects to Which an Order Can Be Linked crm.enum.getorderownertypes

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- no response in case of error
- no response in case of success
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

## Description

```js
crm.enum.getorderownertypes()
```

This method returns the identifiers of the entity types to which an order can be linked.

## Possible Values

{% note info "Note" %}

Currently, an [order link](../../universal/order-entity/crm-order-entity-add.md) can only be made to a **Deal**.

{% endnote %}

```
"result": [
    {
     "attribute": "DYN",
     "code": "DEAL",
     "id": "2",
     "name": "Deal"
}
]
```

## Parameters

No parameters.

## Examples

```javascript
BX24.callMethod(
    "crm.enum.getorderownertypes",
    {},
    function(result)
    {
        if(result.error())
            console.error(result.error());
        else
            console.dir(result.data());
    }
);    
```

{% include [Footnote on examples](../../../../_includes/examples.md) %}