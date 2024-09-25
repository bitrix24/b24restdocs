# Update Inventory catalog.store.update

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- required parameters are not specified
- no response in case of error 
- no examples in other languages
- clarify the parameter type id
  
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

{% include [Parameter notes](../../../_includes/required.md) %}

## Examples

{% list tabs %}

- js
  
    ```
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
                'address': 'Main St. 52',
                'phone': '+1 123 456 789',
                'schedule': 'Mon.-Fri. from 9:00 AM to 8:00 PM, Sat.-Sun. from 11:00 AM to 6:00 PM',
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

- php

    ```
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
                'address' => 'Main St. 52',
                'phone' => '+1 123 456 789',
                'schedule' => 'Mon.-Fri. from 9:00 AM to 8:00 PM, Sat.-Sun. from 11:00 AM to 6:00 PM',
                'xmlId' => 'store_1',
            ],
        ]
    );

    echo '<pre>';
    print_r($result);
    echo '</pre>';
    ```

{% endlist %}

{% include [Example notes](../../../_includes/examples.md) %}