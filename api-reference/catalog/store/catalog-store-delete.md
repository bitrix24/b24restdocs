# Delete Warehouse catalog.store.delete

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- required parameters are not specified
- no response in case of error
  
{% endnote %}

{% endif %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: any user

## Description

```http
catalog.store.delete(id)
```

Method for deleting a warehouse. If the operation is successful, `Y` is returned in the response body.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **id** 
[`integer`](../../data-types.md)| Identifier of the warehouse. ||
|#

{% include [Parameter Note](../../../_includes/required.md) %}

## Examples

{% list tabs %}

- js
  
    ```
    BX24.callMethod(
        'catalog.store.delete',
        {
            id: 42,
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
        'catalog.store.delete',
        [
            'id' => 42,
        ]
    );

    echo '<pre>';
    print_r($result);
    echo '</pre>';
    ```

{% endlist %}

{% include [Example Note](../../../_includes/examples.md) %}