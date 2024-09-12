# Update Price Type catalog.priceType.update

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- the requirement of parameters is not specified
- no response in case of error 
- no response in case of success
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: any user

```http
catalog.priceType.update(id, fields)
```

Method to update the fields of the price type.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Identifier of the price type. ||
|| **fields** 
[`object`](../../data-types.md)|  Fields corresponding to the available list of fields [`fields`](catalog-price-type-get-fields.md). ||
|#

{% include [Note on parameters](../../../_includes/required.md) %}

## Examples

```javascript
BX24.callMethod(
    'catalog.priceType.update',
    {
        id: 14,
        fields: {
            name: "Wholesale Price",
            base: "N",
            sort: 10,
            xmlId: "wholesale"
        }
    },
    function(result)
    {
        if(result.error())
            console.error(result.error().ex);
        else
            console.log(result.data());
    }
);
```
{% include [Note on examples](../../../_includes/examples.md) %}