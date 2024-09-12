# Get VAT Rate Fields catalog.vat.getFields

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- no response in case of error
- no response in case of success
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: any user

## Description

```js
catalog.vat.getFields()
```

This method returns the fields of the VAT rate.

## Parameters

No parameters.

## Examples

```js
BX24.callMethod(
    'catalog.vat.getFields',
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

{% include [Footnote about examples](../../../_includes/examples.md) %}

## Returned Fields

#|
|| **Field** | **Description** | **Note** ||
|| **active** 
[`char`](../../data-types.md) | Activity. | ||
|| **id** 
[`integer`](../../data-types.md) | VAT rate identifier. | Read-only. ||
|| **name^*^** 
[`string`](../../data-types.md) | Name. |  ||
|| **rate^*^** 
[`double`](../../data-types.md) | VAT rate amount. |  ||
|| **sort**
[`integer`](../../data-types.md) | Sorting.  | ||
|| **timestampX**
[`datetime`](../../data-types.md) | Date of modification.  | ||
|#

{% include [Footnote about parameters](../../../_includes/required.md) %}