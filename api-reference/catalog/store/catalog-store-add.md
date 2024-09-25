# Add Inventory catalog.store.add

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

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

This method adds an inventory.
If the operation is successful, it returns the `id` of the added inventory.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **fields**
[`array`](../../data-types.md)| Parameters of the inventory being added. ||
|#

{% include [Notes on parameters](../../../_includes/required.md) %}

## Examples

{% list tabs %}

- js
  
    ```
    BX24.callMethod(
        'catalog.store.add',
        {
            fields: {
                'title': 'Inventory 1',
                'sort': '100',
                'active': 'Y',
                'issuingCenter': 'Y',
                'shippingCenter': 'Y',
                'code': 'store_1',
                'address': 'Main St. 52',
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

- php
  
    ```
    $result = CRest::call(
        'catalog.store.add',
        [
            'fields' => [
                'title' => 'Inventory 1',
                'sort' => '100',
                'active' => 'Y',
                'issuingCenter' => 'Y',
                'shippingCenter' => 'Y',
                'code' => 'store_1',
                'address' => 'Main St. 52',
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

{% include [Notes on examples](../../../_includes/examples.md) %}