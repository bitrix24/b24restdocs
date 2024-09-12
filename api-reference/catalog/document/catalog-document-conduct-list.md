# Execute bulk processing of warehouse accounting documents catalog.document.conductList

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- required parameters are not specified
- no response in case of error
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can subscribe: any user

## Description

```http
catalog.document.conductList(documentIds)
```

Method for bulk processing of warehouse accounting documents.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **documentIds**
[`array`](../../data-types.md)| An array of document identifiers that need to be processed. ||
|#

{% include [Note on parameters](../../../_includes/required.md) %}

## Examples

```js
BX24.callMethod(
    'catalog.document.conductList',
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

{% include [Note on examples](../../../_includes/examples.md) %}