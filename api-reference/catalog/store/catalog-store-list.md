# Get a List of Warehouses by Filter catalog.store.list

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- the requiredness of parameters is not specified
- no response in case of error
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: any user

```http
catalog.store.list(select, filter, order, start)
```

This method retrieves a list of warehouses based on the filter.

If the operation is successful, a list of warehouses is returned in the response body.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **select** 
[`object`](../../data-types.md)| Fields corresponding to the available list of fields [`fields`](catalog-store-get-fields.md).
{% note info "Note" %}

The `select` parameter can be either an object or an array: `select: {0: 'id'}` or `select: ['id']`

{% endnote %}
||
|| **filter** 
[`object`](../../data-types.md)| Fields corresponding to the available list of fields [`fields`](catalog-store-get-fields.md). ||
|| **order**
[`object`](../../data-types.md)| Fields corresponding to the available list of fields [`fields`](catalog-store-get-fields.md). ||
|| **start** 
[`string`](../../data-types.md)| Page number for output. Works for HTTPS requests. ||
|#

{% include [Parameter Note](../../../_includes/required.md) %}

## Examples

For JS

```javascript
BX24.callMethod(
    'catalog.store.list',
    {
        select:['id', 'active'],
        filter:{
            modifiedBy: 1
        },
        order:{
            id: 'ASC'
        },
        start: 1
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

Example of HTTPS request

```
https://your_account/rest/catalog.store.list?auth=_authorization_key_&start=50
```

{% include [Examples Note](../../../_includes/examples.md) %}