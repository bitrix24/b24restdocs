# Get a list of section settings for properties catalog.productPropertySection.list

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- required parameters are not specified
- no response in case of an error
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: any user

```http
catalog.productPropertySection.list(select, filter, order, start)
```

The method retrieves a list of section settings for product properties or variations based on the filter.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **select** 
[`object`](../../data-types.md)| Fields corresponding to the available list of fields [`fields`](catalog-product-property-section-set.md), as well as **propertyId** (identifier of the product or variation property) ||
|| **filter** 
[`object`](../../data-types.md)| Fields corresponding to the available list of fields [`fields`](catalog-product-property-section-set.md), as well as **propertyId** (identifier of the product or variation property) ||
|| **order**
[`object`](../../data-types.md)| Fields corresponding to the available list of fields [`fields`](catalog-product-property-section-set.md), as well as **propertyId** (identifier of the product or variation property) ||
|| **start** 
[`string`](../../data-types.md)| Page number for output. Works for HTTPS requests ||
|#

{% include [Note on parameters](../../../_includes/required.md) %}

## Examples

{% list tabs %}

- JS

    ```js
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

{% endlist %}

Example HTTPS request

```
https://your_account/rest/catalog.productPropertySection.list?auth=_authorization_key_&start=50
```

{% include [Note on examples](../../../_includes/examples.md) %}