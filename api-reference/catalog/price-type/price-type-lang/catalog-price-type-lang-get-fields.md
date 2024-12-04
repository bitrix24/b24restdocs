# Get Fields for Price Type Translation catalog.priceTypeLang.getFields

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

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

## Description

```js
catalog.priceTypeLang.getFields()
```

The method returns the fields for the translation of the price type name.

## Parameters

No parameters.

## Examples

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'catalog.priceTypeLang.getFields',
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

{% include [Footnote on examples](../../../../_includes/examples.md) %}

## Returned Fields

#|
|| **Field** | **Description** | **Note** ||
|| **catalogGroupId^*^** 
[`integer`](../../data-types.md) | ID of the price type |  ||
|| **id**
[`integer`](../../data-types.md) | Identifier for the translation of the price type name | Immutable ||
|| **lang^*^**
[`string`](../../data-types.md) | Language of the translation |  ||
|| **name^*^**
[`string`](../../data-types.md) | Name of the price type |  ||
|#

{% include [Footnote on parameters](../../../../_includes/required.md) %}