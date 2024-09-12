# Get Available Product or Deal Property Parameters catalog.productPropertyFeature.getAvailableFeaturesByProperty

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- the requirement of parameters is not specified
- no response in case of an error
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: any user

## Description

```http
catalog.productPropertyFeature.getAvailableFeaturesByProperty(propertyId)
```

This method removes the values of list properties. If the operation is successful, it returns `true` in the response body.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **propertyId** 
[`integer`](../../data-types.md)| The identifier of the product or deal property. ||
|#

{% include [Note on parameters](../../../_includes/required.md) %}

## Examples

```javascript
BX24.callMethod(
    'catalog.productPropertyFeature.getAvailableFeaturesByProperty',
    {
        propertyId: 128
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