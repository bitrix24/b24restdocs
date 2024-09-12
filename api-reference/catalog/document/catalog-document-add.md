# Add Inventory Document catalog.document.add

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- the mandatory parameters are not specified
- there is no response in case of an error
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can subscribe: any user

## Description

```http
catalog.document.add(fields)
```

This method adds an [`inventory document`](../enum/catalog-enum-get-store-document-types.md).
If the operation is successful, it returns the `id` of the added document.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **fields**
[`array`](../../data-types.md)| Parameters of the document being added. The fields correspond to the available list of fields [`fields`](catalog-document-get-fields.md). ||
|#

{% include [Parameter Note](../../../_includes/required.md) %}

## Examples

{% list tabs %}

- js
  
    ```
    BX24.callMethod(
        'catalog.document.element.update',
        {
            'id': 42,
            'fields': {
                'amount': 10,
                'purchasingPrice': 25,
            }
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

- php
  
    ```
    $result = CRest::call(
        'catalog.document.element.update',
        [
            'id' => 11,
            'fields' => [
                'amount' => 10,
                'purchasingPrice' => 25,
            ],
        ]
    );

    echo '<pre>';
    print_r($result);
    echo '</pre>';
    ```

{% endlist %}

{% include [Examples Note](../../../_includes/examples.md) %}