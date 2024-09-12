# Change the values of product property feature parameters or trade offers catalog.productPropertyFeature.update

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- the mandatory nature of parameters is not specified
- no response in case of error
- no response in case of success
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: any user

```http
catalog.productPropertyFeature.update(id, fields)
```

Method for updating the values of product property feature parameters or trade offers.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Identifier of the product property feature or trade offer. ||
|| **fields** 
[`object`](../../data-types.md)|  Fields corresponding to the available list of fields [`fields`](catalog-product-property-feature-get-fields.md). ||
|#

{% include [Note on parameters](../../../_includes/required.md) %}

## Examples

```javascript
BX24.callMethod(
    'catalog.productPropertyFeature.update',
    {
        id: 144,
        fields: {
            propertyId: 128,
            featureId: "IN_BASKET",
            moduleId: "catalog",
            isEnabled: "Y"
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