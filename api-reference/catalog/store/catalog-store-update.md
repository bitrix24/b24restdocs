# Update Inventory catalog.store.update

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly

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
> Who can execute the method: any user

```http
catalog.store.update(id, fields)
```

Method for updating the inventory.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **id**
[`string`](../../data-types.md) | Identifier of the inventory. ||
|| **fields** 
[`array`](../../data-types.md)|  Parameters of the updated inventory. ||
|#

{% include [Footnote about parameters](../../../_includes/required.md) %}

## Examples

{% list tabs %}

- JS
  
    ```js
    BX24.callMethod(
        'catalog.store.update',
        {
            id: 42,
            fields: {
                'title': 'Inventory 1',
                'sort': '100',
                'active': 'Y',
                'issuingCenter': 'Y',
                'shippingCenter': 'Y',
                'code': 'store_1',
                'address': 'Moscow Ave. 52',
                'phone': '+1 123 456 789',
                'schedule': 'Mon.-Fri. from 9:00 to 20:00, Sat.-Sun. from 11:00 to 18:00',
                'xmlId': 'store_1',
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
        'catalog.store.update',
        [
            'id' => 42,
            'fields' => [
                'title' => 'Inventory 1',
                'sort' => '100',
                'active' => 'Y',
                'issuingCenter' => 'Y',
                'shippingCenter' => 'Y',
                'code' => 'store_1',
                'address' => 'Moscow Ave. 52',
                'phone' => '+1 123 456 789',
                'schedule' => 'Mon.-Fri. from 9:00 to 20:00, Sat.-Sun. from 11:00 to 18:00',
                'xmlId' => 'store_1',
            ],
        ]
    );

    echo '<pre>';
    print_r($result);
    echo '</pre>';
    ```

{% endlist %}

{% include [Footnote about examples](../../../_includes/examples.md) %}