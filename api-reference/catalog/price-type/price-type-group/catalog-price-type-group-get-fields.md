# Get Fields for Price Type Bindings to Customer Groups catalog.priceTypeGroup.getFields

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- no response in case of error
- no response in case of success
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

## Description

```js
catalog.priceTypeGroup.getFields()
```

The method returns the fields for binding price types to customer groups.

## Parameters

No parameters.

## Examples

{% list tabs %}

- JS

    ```javascript
    BX24.callMethod(
        'catalog.priceTypeGroup.getFields',
        {},
        function(result)
        {
            if(result.error())
                console.error(result.error().ex);
            else
                console.log(result.data());
        }
    );
    ```

{% endlist %}

{% include [Footnote about examples](../../../../_includes/examples.md) %}

## Returned Fields

#|
|| **Field** | **Description** | **Note** ||
|| **access^*^** 
[`char`](../../data-types.md) | Type of added binding:
- `N` – permission to view this price type;
- `Y` – permission to purchase at this price type.
To add both permissions, the method must be called twice, sequentially specifying both types of binding (`N` and `Y`). |  ||
|| **catalogGroupId^*^** 
[`integer`](../../data-types.md) | ID of the price type. |  ||
|| **groupId^*^** 
[`integer`](../../data-types.md) | ID of the group to which the price is bound. |  ||
|| **id**
[`integer`](../../data-types.md) | Identifier of the binding. | Immutable field. ||
|# 

{% include [Footnote about parameters](../../../../_includes/required.md) %}