# Get a list of product properties or trade offers catalog.productProperty.list

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- the mandatory parameters are not specified
- there is no response in case of an error
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: any user

```http
catalog.productProperty.list(select, filter, order, start)
```

The method retrieves a list of product properties or trade offers. If the operation is successful, a list of product properties or trade offers is returned in the response body.


## Parameters

#|
|| **Parameter** | **Description** ||
|| **select** 
[`object`](../../data-types.md)| Fields corresponding to the available list of fields [`fields`](catalog-product-property-get-fields.md).||
|| **filter** 
[`object`](../../data-types.md)| Fields corresponding to the available list of fields [`fields`](catalog-product-property-get-fields.md). ||
|| **order**
[`object`](../../data-types.md)| Fields corresponding to the available list of fields [`fields`](catalog-product-property-get-fields.md). ||
|| **start** 
[`string`](../../data-types.md)| Page number for output. Works for HTTPS requests. ||
|#

{% include [Note on parameters](../../../_includes/required.md) %}

## Examples

For JS

```javascript
BX24.callMethod(
    'catalog.productProperty.list',
    {
        select: ['id'],
        filter: {
            iblockId: 16
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
https://your_account/rest/catalog.productProperty.list?auth=_authorization_key_&start=50
```

{% include [Note on examples](../../../_includes/examples.md) %}