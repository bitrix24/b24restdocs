# Delete Inventory Document Item catalog.document.element.delete

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- required parameters are not specified
- no response in case of error
  
{% endnote %}

{% endif %}

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

## Description

```http
catalog.document.element.delete(id)
```

Method for deleting an inventory document item. If the operation is successful, `true` is returned in the response body.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **id** 
[`integer`](../../../data-types.md)| Identifier of the document item. ||
|#

{% include [Parameter Note](../../../../_includes/required.md) %}

## Examples

{% list tabs %}

- js
  
    ```
    BX24.callMethod(
        'catalog.document.element.delete',
        {
            'id': 42,
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
        'catalog.document.element.delete',
        [
            'id' => 42,
        ]
    );

    echo '<pre>';
    print_r($result);
    echo '</pre>';
    ```

{% endlist %}

{% include [Example Note](../../../../_includes/examples.md) %}