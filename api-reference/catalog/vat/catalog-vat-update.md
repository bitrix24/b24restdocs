# Update VAT Rate catalog.vat.update

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- the requiredness of parameters is not specified
- no response in case of error 
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: any user

```http
catalog.vat.update(id, fields)
```

Method for updating the VAT rate.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Identifier of the VAT rate. ||
|| **fields** 
[`object`](../../data-types.md)|  Fields corresponding to the available list of fields [fields](catalog-vat-get-fields.md). ||
|#

{% include [Note on parameters](../../../_includes/required.md) %}

## Examples

```js
BX24.callMethod(
    'catalog.vat.update',
    {
        id: 9,
        fields: {
            name: "Tax 15%",
            rate: 15,
            sort: 20,
            active: "Y"
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