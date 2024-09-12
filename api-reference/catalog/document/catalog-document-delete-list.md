# Execute bulk deletion of inventory documents catalog.document.deleteList

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- the required parameter specifications are missing
- there is no response in case of an error
  
{% endnote %}

{% endif %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can subscribe: any user

## Description

```http
catalog.document.deleteList(documentIds)
```

Method for bulk deletion of inventory documents.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **documentIds** 
[`array`](../../data-types.md)| An array of document identifiers that need to be deleted. ||
|#

{% include [Note on parameters](../../../_includes/required.md) %}

## Examples

```js
BX24.callMethod(
    'catalog.document.deleteList',
    {
        "documentIds": [
            "110",
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