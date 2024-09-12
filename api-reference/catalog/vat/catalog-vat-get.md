# Get VAT Rate Field Values catalog.vat.get

{% note warning "We are still updating this page" %}

Some data may be missing — we will add it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- required parameter specifications are missing
- no response in case of an error
- no response in case of success
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: any user

## Description

```http
catalog.vat.get(id)
```

Method to access the VAT rate field values by ID. If the operation is successful, a [VAT rate resource](resource.md) is returned in the response body.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **id** 
[`integer`](../../data-types.md)| Identifier of the VAT rate. ||
|#

{% include [Parameter Note](../../../_includes/required.md) %}

## Examples

```js
BX24.callMethod(
    'catalog.vat.get',
    {
        id: 7
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

{% include [Examples Note](../../../_includes/examples.md) %}