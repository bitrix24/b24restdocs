# Add Warehouse catalog.store.add

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- required parameters are not specified
- no response in case of error
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: any user

## Description

```http
catalog.store.add(fields)
```

This method adds a warehouse. If the operation is successful, it returns the `id` of the added warehouse.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **fields**
[`array`](../../data-types.md)| Parameters of the warehouse being added. ||
|#

{% include [Parameter Note](../../../_includes/required.md) %}

## Examples

{% list tabs %}

- js
  
    ```
    BX24.callMethod(
        'catalog.store.add',
        {
            fields: {
                'title': 'Warehouse 1',
                'sort': '100',
                'active': 'Y',
                'issuingCenter': 'Y',
                'shippingCenter': 'Y',
                'code': 'store_1',
                'address': 'Moscow Ave. 52',
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
        'catalog.store.add',
        [
            'fields' => [
                'title' => 'Warehouse 1',
                'sort' => '100',
                'active' => 'Y',
                'issuingCenter' => 'Y',
                'shippingCenter' => 'Y',
                'code' => 'store_1',
                'address' => 'Moscow Ave. 52',
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

{% include [Example Note](../../../_includes/examples.md) %}