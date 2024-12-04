# Execute bulk processing of inventory management documents catalog.document.conductList

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly

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

Method for bulk processing of inventory management documents.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **documentIds**
[`array`](../../data-types.md)| Array of document identifiers to be processed. ||
|#

{% include [Footnote about parameters](../../../_includes/required.md) %}

## Examples

{% list tabs %}

- JS

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

{% endlist %}

{% include [Footnote about examples](../../../_includes/examples.md) %}