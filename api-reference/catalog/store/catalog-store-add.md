# Add Inventory catalog.store.add

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

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
If the operation is successful, the `id` of the added inventory is returned.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **fields**
[`array`](../../data-types.md)| Parameters of the inventory being added. ||
|#

{% include [Footnote about parameters](../../../_includes/required.md) %}

## Examples

{% list tabs %}

- JS
  
    ```js
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

- PHP
  
    ```php
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

{% include [Footnote about examples](../../../_includes/examples.md) %}