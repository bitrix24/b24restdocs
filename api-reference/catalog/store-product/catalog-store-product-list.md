# Get Inventory Balances by Warehouse catalog.storeproduct.list

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

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
catalog.storeproduct.list(select, filter, order, start)
```

The method retrieves inventory balances by warehouse, filtered according to the specified criteria.

If the operation is successful, a list of inventory balances by warehouse is returned in the response body.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **select** 
[`object`](../../data-types.md)| Fields corresponding to the available list of fields [`fields`](catalog-store-product-get-fields.md).||
|| **filter** 
[`object`](../../data-types.md)| Fields corresponding to the available list of fields [`fields`](catalog-store-product-get-fields.md). ||
|| **order**
[`object`](../../data-types.md)| Fields corresponding to the available list of fields [`fields`](catalog-store-product-get-fields.md). ||
|| **start** 
[`string`](../../data-types.md)| Page number for output. Works for HTTPS requests. ||
|#

{% include [Parameter Notes](../../../_includes/required.md) %}

## Examples

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'catalog.storeproduct.list',
        {
            select: {
                id
            },
            filter: {
                productId: 8
            },
        },
        function(result) {
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
https://your_account/rest/catalog.storeproduct.list?auth=_authorization_key_&start=50
```

{% include [Example Notes](../../../_includes/examples.md) %}