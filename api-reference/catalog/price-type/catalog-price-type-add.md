# Add Price Type catalog.priceType.add

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- required parameters are not specified
- no response in case of success
- no response in case of error
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: any user

## Description

```http
catalog.priceType.add(fields)
```

This method adds a price type.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **fields**
[`object`](../../data-types.md)| Fields corresponding to the available list of fields [`fields`](catalog-price-type-get-fields.md). ||
|#

{% include [Note on parameters](../../../_includes/required.md) %}

## Examples

```javascript
BX24.callMethod(
    'catalog.priceType.add',
    {
        fields: {
            name: "Wholesale Price",
            base: "N",
            sort: 10,
            xmlId: "wholesale"
        }
    },
    function(result) {
        if (result.error())
            console.error(result.error().ex);
        else
            console.log(result.data());
    }
);
```
{% include [Note on examples](../../../_includes/examples.md) %}