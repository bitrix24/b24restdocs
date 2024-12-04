# Get Inventory Balances Fields catalog.storeproduct.getFields

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- no response in case of error
- no response in case of success
- required fields are not specified
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: any user

## Description

```js
catalog.storeproduct.getFields()
```

This method returns the fields for inventory balances.

## Parameters

No parameters.

## Examples

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'catalog.storeproduct.getFields',
        {},
        function(result) {
            if(result.error())
                console.error(result.error().ex);
            else
                console.log(result.data());
        }
    );
    ```

{% endlist %}

{% include [Footnote about examples](../../../_includes/examples.md) %}

## Returned Fields

#|
|| **Field** | **Description** | **Note** ||
|| **amount** 
[`double`](../../data-types.md) | Quantity | Read-only. ||
|| **id** 
[`integer`](../../data-types.md) | Primary key of the record | Read-only. ||
|| **productId** 
[`integer`](../../data-types.md) | Identifier of the product or variation | Read-only. ||
|| **quantityReserved** 
[`double`](../../data-types.md) | Reserved quantity | Read-only. ||
|| **storeId** 
[`integer`](../../data-types.md) | Identifier of the inventory | Read-only. ||
|#

{% include [Footnote about parameters](../../../_includes/required.md) %}