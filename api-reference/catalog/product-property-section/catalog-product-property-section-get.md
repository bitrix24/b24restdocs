# Get Values of Section Settings for catalog.productPropertySection.get

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- Required parameters are not specified
- No response in case of an error
- No response in case of success
- No examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: any user

## Description

```http
catalog.productPropertySection.get(propertyId)
```

This method is used to access the value of section settings for product properties or trade offers.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **propertyId** 
[`integer`](../../data-types.md)| The identifier of the product property or trade offer. ||
|#

{% include [Parameter Note](../../../_includes/required.md) %}

## Examples

```javascript
BX24.callMethod(
    'catalog.productPropertySection.get',
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
{% include [Examples Note](../../../_includes/examples.md) %}