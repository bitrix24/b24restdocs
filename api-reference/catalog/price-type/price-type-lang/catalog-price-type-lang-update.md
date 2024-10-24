# Update Translation of Price Type Name catalog.priceTypeLang.update

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- required parameters are not specified
- no response in case of error
- no response in case of success
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

```http
catalog.priceTypeLang.update(id, fields)
```

Method for updating the translation of the price type name.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Identifier of the price type name translation. ||
|| **fields** 
[`object`](../../data-types.md)|  Fields corresponding to the available list of fields [`fields`](./catalog-price-type-lang-get-fields.md). ||
|#

{% include [Notes on parameters](../../../../_includes/required.md) %}

## Examples

```javascript
BX24.callMethod(
    'catalog.priceTypeLang.update',
    {
        id: 537,
        fields: {            
            catalogGroupId: 14,
            lang: "de",
            name: "Wholesale price for partners"
        }
    },
    function(result)
    {
        if(result.error())
            console.error(result.error().ex);
        else
            console.log(result.data());
    }
);
```
{% include [Notes on examples](../../../../_includes/examples.md) %}