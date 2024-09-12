# Get a list of price type bindings to customer groups catalog.priceTypeGroup.list

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- required parameter specification is missing
- no response in case of error
- no response in case of success
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: any user

```http
catalog.priceTypeGroup.list(select, filter, order, start)
```

This method retrieves a list of price type bindings to customer groups.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **select** 
[`object`](../../data-types.md)| Fields corresponding to the available list of fields [`fields`](catalog-price-type-group-get-fields.md).||
|| **filter** 
[`object`](../../data-types.md)| Fields corresponding to the available list of fields [`fields`](catalog-price-type-group-get-fields.md). ||
|| **order**
[`object`](../../data-types.md)| Fields corresponding to the available list of fields [`fields`](catalog-price-type-group-get-fields.md). ||
|| **start** 
[`string`](../../data-types.md)| Page number for output. Works for HTTPS requests. ||
|#

{% include [Parameter notes](../../../_includes/required.md) %}

## Examples

For JS

```javascript
BX24.callMethod(
    'catalog.priceTypeGroup.list',
    {
        select: ['catalogGroupId'],
        filter: {
            groupId: 8
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
https://your_account/rest/catalog.priceTypeGroup.list?auth=_authorization_key_&start=50
```

{% include [Example notes](../../../_includes/examples.md) %}