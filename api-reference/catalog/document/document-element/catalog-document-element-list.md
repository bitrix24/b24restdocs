# Get a List of Products in Inventory Management Documents catalog.document.element.list

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- required parameters are not specified
- no response in case of an error
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

```
catalog.document.element.list(order, filter, select, offset, limit, start)
```

This method retrieves a list of products in inventory management documents.

If the operation is successful, a list of documents is returned in the response body.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **order**
[`array`](../../../data-types.md)| Sorting. ||
|| **filter** 
[`array`](../../../data-types.md)| Filter. ||
|| **select** 
[`array`](../../../data-types.md)| List of fields to retrieve. ||
|| **offset** 
[`integer`](../../../data-types.md)| Page offset, works similarly to start. ||
|| **start** 
[`string`](../../../data-types.md)| Page number for output. Works for HTTPS requests. ||
|| **limit** 
[`integer`](../../../data-types.md)| Page size from 1 to 500 (if 0 or a number greater than the maximum is specified, the default value of 50 is used). ||
|#

{% include [Parameter Notes](../../../../_includes/required.md) %}

## Examples

{% list tabs %}

- js

    ```
    BX24.callMethod(
        'catalog.document.element.list',
        {
            'order': {
                'id': 'asc',
            },
            'filter': {
                '>id': 0,
            },
            'offset': 0,
            'limit': 50,
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
        'catalog.document.element.list',
        [
            'order' => [
                'id' => 'asc',
            ],
            'filter' => [
                '>id' => 0,
            ],
            'offset' => 0,
            'limit' => 5,
        ]
    );

    echo '<pre>';
    print_r($result);
    echo '</pre>';
    ```

- For HTTPS
  
```
https://your_account/rest/catalog.document.element.list?auth=_authorization_key_&start=50
```

{% endlist %}

{% include [Example Notes](../../../../_includes/examples.md) %}