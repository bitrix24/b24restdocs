# Add VAT Rate catalog.vat.add

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- required parameters are not specified
- no response in case of error and success
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: any user

## Description

```http
catalog.vat.add(fields)
```

This method adds a VAT rate.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **fields**
[`object`](../../data-types.md)| Fields corresponding to the available list of fields [fields](catalog-vat-get-fields.md). ||
|#

{% include [Note on parameters](../../../_includes/required.md) %}

## Examples

```js
BX24.callMethod(
    'catalog.vat.add',
    {
        fields: {
            name: "Tax 13%",
            rate: 13,
            sort: 10,
            active: "Y"
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