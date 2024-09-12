# Add Translation for Price Type Title catalog.priceTypeLang.add

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- required parameters are not specified
- no response in case of success
- no response in case of error
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: any user

## Description

```http
catalog.priceTypeLang.add(fields)
```

This method adds a translation for the price type title.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **fields**
[`object`](../../data-types.md)| Fields corresponding to the available list of fields [`fields`](catalog-price-type-lang-get-fields.md). ||
|#

{% include [Note on parameters](../../../_includes/required.md) %}

## Examples

```javascript
BX24.callMethod(
    'catalog.priceTypeLang.add',
    {
        fields: {
            catalogGroupId: 14,
            lang: "en",
            name: "Wholesale Price"
        }
    },
    function(result) {
        if (result.error())
            console.error(result.error().ex);
        else
            console.log(result.data());
    }
);
```
{% include [Note on examples](../../../_includes/examples.md) %}