# Delete VAT Rate catalog.vat.delete

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- the requiredness of parameters is not specified
- no response in case of error
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: any user

## Description

```http
catalog.vat.delete(id)
```

Method for deleting a VAT rate. If the operation is successful, `true` is returned in the response body.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **id** 
[`integer`](../../data-types.md)| Identifier of the VAT rate. ||
|#

{% include [Footnote about parameters](../../../_includes/required.md) %}

## Examples

```js
BX24.callMethod(
    'catalog.vat.delete',
    {
        id: 11
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

{% include [Footnote about examples](../../../_includes/examples.md) %}