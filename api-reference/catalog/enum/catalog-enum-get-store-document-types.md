# Get the types of inventory management documents available for REST catalog.enum.getStoreDocumentTypes

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

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
catalog.enum.getStoreDocumentTypes()
```

This method returns the types of inventory management documents available for REST.

Currently, the following types are available:
- `A` – Stock receipt of goods;
- `S` – Stock adjustment of goods;
- `M` – Transfer of goods between inventories;
- `R` – Return of goods;
- `D` – Write-off of goods.

## Parameters

No parameters.

## Examples

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'catalog.enum.getStoreDocumentTypes',
        {},
        function(result)
        {
            if(result.error())
                console.error(result.error().ex);
            else
                console.log(result.data());
        }
    );
    ```

{% endlist %}

{% include [Examples note](../../../_includes/examples.md) %}