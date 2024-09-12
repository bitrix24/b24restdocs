# Conduct Warehouse Accounting Document catalog.document.conduct

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- required parameter specifications are missing
- no response in case of an error
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can subscribe: any user

## Description

```http
catalog.document.conduct(id)
```

Method for conducting a warehouse accounting document. If the operation is successful, `true` is returned in the response body.


## Parameters

#|
|| **Parameter** | **Description** ||
|| **id**
[`integer`](../../data-types.md)| Identifier of the warehouse accounting document. ||
|#

{% include [Parameter Note](../../../_includes/required.md) %}

## Examples

```js
BX24.callMethod(
    'catalog.document.conduct',
    {
        id: 112
    },
    function(result)
    {
        if(result.error())
            console.error(result.error());
        else
            console.log(result.data());
    }
);
```

{% include [Examples Note](../../../_includes/examples.md) %}