# Add Inventory Document Item catalog.document.element.add

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

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

## Description

```http
catalog.document.element.add(fields)
```

This method adds an item to the inventory document. If the operation is successful, the `id` of the added item is returned.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **fields**
[`array`](../../../data-types.md)| Parameters of the item being added. The fields correspond to the available list of fields [`fields`](catalog-document-element-get-fields.md) ||
|#

{% include [Notes on parameters](../../../../_includes/required.md) %}

## Examples

{% list tabs %}

- js
  
    ```
    BX24.callMethod(
        'catalog.document.element.add',
        {
            'fields': {
                'docId': 1,
                'storeFrom': 0,
                'storeTo': 1,
                'elementId': 42,
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
        'catalog.document.element.add',
        [
            'fields' => [
                'docId' => 1,
                'storeFrom' => 0,
                'storeTo' => 1,
                'elementId' => 42,
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

{% include [Notes on examples](../../../../_includes/examples.md) %}