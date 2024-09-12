# Update Translation of Price Type Title catalog.priceTypeLang.update

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- Required parameters are not specified
- No response in case of error
- No response in case of success
- No examples in other languages

{% endnote %}

{% endif %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: any user

```http
catalog.priceTypeLang.update(id, fields)
```

Method for updating the translation of the price type title.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Identifier of the price type title translation. ||
|| **fields** 
[`object`](../../data-types.md)|  Fields corresponding to the available list of fields [`fields`](catalog-price-type-lang-get-fields.md). ||
|#

{% include [Note on parameters](../../../_includes/required.md) %}

## Examples

```javascript
BX24.callMethod(
    'catalog.priceTypeLang.update',
    {
        id: 537,
        fields: {            
            catalogGroupId: 14,
            lang: "en",
            name: "Wholesale Price for Partners"
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
{% include [Note on examples](../../../_includes/examples.md) %}