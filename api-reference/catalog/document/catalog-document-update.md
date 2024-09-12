# Update Warehouse Accounting Document catalog.document.update

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- the requiredness of parameters is not specified
- no response in case of error
- no examples in other languages
- clarify the type of the parameter id

{% endnote %}

{% endif %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can subscribe: any user

```http
catalog.document.update(id, fields)
```

Method for updating a warehouse accounting document. If the operation is successful, it returns `true` for the added warehouse.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **id**
[`string`](../../data-types.md) | Identifier of the document. ||
|| **fields** 
[`array`](../../data-types.md)|  Document parameters. ||
|#

{% include [Parameter Note](../../../_includes/required.md) %}

## Examples

{% list tabs %}

- js
  
    ```
    BX24.callMethod(
        'catalog.document.update',
        {
            'id': 42,
            'fields': {
                'total': '1000', // total amount of all PURCHASING_PRICE multiplied by AMOUNT
                'commentary': 'first document.',
                'title': 'New Document', //title (field available since catalog version 22.200.0)
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
        'catalog.document.update',
        [
            'id' => 42,
            'fields' => [
                'total' => '1000',
                'commentary' => 'first document.',
                'title' => 'New Document',
            ],
        ]
    );
    echo '<pre>';
    print_r($result);
    echo '</pre>';
    ```

{% endlist %}

{% include [Examples Note](../../../_includes/examples.md) %}