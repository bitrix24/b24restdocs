# Get Fields of List Property Values catalog.productPropertyEnum.getFields

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

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
catalog.productPropertyEnum.getFields()
```

The method returns the fields of list property values.

## Parameters

No parameters.

## Examples

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'catalog.productPropertyEnum.getFields',
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


{% include [Footnote on examples](../../../_includes/examples.md) %}

## Returned Fields

#|
|| **Field** 
[`Type`](../../data-types.md) | **Description** | **Note** ||
|| **def** 
[`char`](../../data-types.md) | Indicates if it is the default value. | ||
|| **id** 
[`integer`](../../data-types.md) | Identifier of the value. | Read-only. ||
|| **propertyId^*^** 
[`integer`](../../data-types.md) | Identifier of the property. |  ||
|| **sort** 
[`integer`](../../data-types.md) | Sorting. | ||
|| **value^*^** 
[`string`](../../data-types.md) | Value. |  ||
|| **xmlId^*^** 
[`string`](../../data-types.md) | External identifier. | ||
|#
{% include [Footnote on parameters](../../../_includes/required.md) %}