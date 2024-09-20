# Cancel the Inventory Document catalog.document.cancel

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- Required parameters are not specified
- No response in case of an error
- No examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can subscribe: any user

## Description

```http
catalog.document.cancel(id)
```

This method cancels the processing of the document by ID.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **id**
[`integer`](../../data-types.md)| Identifier of the inventory document. ||
|#

{% include [Note on parameters](../../../_includes/required.md) %}

## Examples

```js
BX24.callMethod(
    'catalog.document.cancel',
    {
        id: 112
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