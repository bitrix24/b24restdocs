# Update Inventory Management Document catalog.document.update

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- required parameters are not specified
- no response in case of error
- no examples in other languages
- clarify the type of the id parameter
  
{% endnote %}

{% endif %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can subscribe: any user

```http
catalog.document.update(id, fields)
```

Method for updating an inventory management document. If the operation is successful, it returns `true` for the added inventory.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **id**
[`string`](../../data-types.md) | Document identifier. ||
|| **fields** 
[`array`](../../data-types.md)|  Document parameters. ||
|#

{% include [Footnote about parameters](../../../_includes/required.md) %}

## Examples

{% list tabs %}

- JS
  
    ```js
    BX24.callMethod(
        'catalog.document.update',
        {
            'id': 42,
            'fields': {
                'total': '1000', // total amount of all PURCHASING_PRICE multiplied by AMOUNT
                'commentary': 'first document.',
                'title': 'New document', //title (field available since catalog version 22.200.0)
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

- PHP

    ```php
    $result = CRest::call(
        'catalog.document.update',
        [
            'id' => 42,
            'fields' => [
                'total' => '1000',
                'commentary' => 'first document.',
                'title' => 'New document',
            ],
        ]
    );
    echo '<pre>';
    print_r($result);
    echo '</pre>';
    ```

{% endlist %}

{% include [Footnote about examples](../../../_includes/examples.md) %}