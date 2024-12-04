# Get Values of Section Settings for catalog.productPropertySection.get

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- required parameters are not specified
- no response in case of error
- no response in case of success
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: any user

## Description

```http
catalog.productPropertySection.get(propertyId)
```

Method to access the value of section settings for product properties or variations.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **propertyId** 
[`integer`](../../data-types.md)| Identifier of the product property or variation. ||
|#

{% include [Note on parameters](../../../_includes/required.md) %}

## Examples

{% list tabs %}

- JS

```js
BX24.callMethod(
    'catalog.productPropertySection.get',
    {
        propertyId: 128
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

{% endlist %}

{% include [Note on examples](../../../_includes/examples.md) %}