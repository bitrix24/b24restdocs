# Get a list of section settings for properties catalog.productPropertySection.list

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- Required parameters are not specified
- No response in case of an error
- No examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: any user

```http
catalog.productPropertySection.list(select, filter, order, start)
```

This method retrieves a list of section settings for product properties or trade offers based on the filter.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **select** 
[`object`](../../data-types.md)| Fields corresponding to the available list of fields [`fields`](catalog-product-property-section-set.md), as well as **propertyId** (identifier of the product or trade offer property).||
|| **filter** 
[`object`](../../data-types.md)| Fields corresponding to the available list of fields [`fields`](catalog-product-property-section-set.md), as well as **propertyId** (identifier of the product or trade offer property). ||
|| **order**
[`object`](../../data-types.md)| Fields corresponding to the available list of fields [`fields`](catalog-product-property-section-set.md), as well as **propertyId** (identifier of the product or trade offer property). ||
|| **start** 
[`string`](../../data-types.md)| Page number for output. Works for HTTPS requests. ||
|#

{% include [Note on parameters](../../../_includes/required.md) %}

## Examples

For JS

```javascript
BX24.callMethod(
    'catalog.productPropertySection.list',
    {
        filter:{
            propertyId: 128
        },
    },
    function(result)
    {
        if(result.error())
            console.error(result.error().ex);
        else
            console.log(result.data());
        result.next();
    }
);
```

Example HTTPS request

```
https://your_account/rest/catalog.productPropertySection.list?auth=_authorization_key_&start=50
```

{% include [Note on examples](../../../_includes/examples.md) %}