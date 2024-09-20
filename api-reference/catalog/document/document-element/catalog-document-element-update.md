# Update Inventory Document Item catalog.document.element.update

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- the mandatory parameters are not specified
- there is no response in case of an error
- no examples in other languages
- clarify the parameter type id

{% endnote %}

{% endif %}

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

```http
catalog.document.element.update(id, fields)
```

Method for updating an inventory document item. If the operation is successful, it returns `true`.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Identifier of the item being updated. ||
|| **fields** 
[`array`](../../../data-types.md)|  Parameters of the item being updated. ||
|#

{% include [Notes on parameters](../../../../_includes/required.md) %}

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

{% include [Notes on examples](../../../../_includes/examples.md) %}