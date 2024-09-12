# Get Margin Field Values by ID catalog.extra.get

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

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
catalog.extra.get(id)
```

Method to access the margin field values by ID. If the operation is successful, a [margin resource](resource.md) is returned in the response body.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **id** 
[`string`](../../data-types.md)| Margin number. ||
|#

{% include [Note on parameters](../../../_includes/required.md) %}

## Examples

```javascript
BX24.callMethod(
    'catalog.extra.get',
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
{% include [Note on examples](../../../_includes/examples.md) %}