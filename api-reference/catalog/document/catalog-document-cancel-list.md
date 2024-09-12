# Execute a Group Cancellation of Warehouse Accounting Documents catalog.document.cancelList

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- the requiredness of parameters is not specified
- there is no response in case of an error
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can subscribe: any user

## Description

```http
catalog.document.cancelList(documentIds)
```

Method for group cancellation of warehouse accounting documents.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **documentIds**
[`array`](../../data-types.md)| An array of document identifiers for which the cancellation is required. ||
|#

{% include [Notes on parameters](../../../_includes/required.md) %}

## Examples

```js
BX24.callMethod(
    'catalog.document.cancelList',
    {
        "documentIds": [
            "114",
            "112"
        ]
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

{% include [Notes on examples](../../../_includes/examples.md) %}