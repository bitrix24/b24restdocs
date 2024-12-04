# Update Product of Inventory Document catalog.document.element.update

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- required parameters are not specified
- no response in case of error 
- no examples in other languages
- clarify the type of the id parameter
  
{% endnote %}

{% endif %}

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

```http
catalog.document.element.update(id, fields)
```

Method for updating a product of the inventory document.
If the operation is successful, it returns `true`.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Identifier of the product being updated. ||
|| **fields** 
[`array`](../../../data-types.md)|  Parameters of the product being updated. ||
|#

{% include [Footnote about parameters](../../../../_includes/required.md) %}

## Examples

{% list tabs %}

- js
  
    ```js
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

    ```php
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

{% include [Footnote about examples](../../../../_includes/examples.md) %}