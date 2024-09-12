# Remove Order Binding to CRM Object crm.orderentity.deleteByFilter

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: administrator of the online store

This method removes the binding of an order to a CRM object.

## Method Parameters

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../../data-types.md) | Field values for removing the binding ||
|#

### Parameter fields

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **orderId***
[`sale_order.id`](../../../sale/data-types.md#sale_order) | Order identifier ||
|| **ownerTypeId***
[`integer`](../../../data-types.md) | Identifier of the [CRM object type](../../data-types.md#object_type).

Binding is only possible to a deal or invoice
||
|| **ownerId***
[`integer`](../../../data-types.md) | Identifier of the CRM object.

For deals, it can be obtained using the [crm.deal.list](../../deals/crm-deal-list.md) method.

For invoices, it can be obtained using the [crm.invoice.list](../../outdated/invoice/crm-invoice-list.md) method
||
|#

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

Add order binding to a deal:

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"orderId":5125,"ownerId":6933,"ownerTypeId":2}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.orderentity.deletebyfilter
    ```

- cURL (OAuth) 

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"orderId":5125,"ownerId":6933,"ownerTypeId":2},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.orderentity.deletebyfilter
    ```

- JS

    ```js
    BX24.callMethod(
        "crm.orderentity.deletebyfilter",
        {
            fields: {
                orderId: 5125,
                ownerId: 6933,
                ownerTypeId: 2
            }
        },
    )
        .then(
            function(result)
            {
                if (result.error())
                {
                    console.error(result.error());
                }
                else
                {
                    console.log(result);
                }
            },
            function(error)
            {
                console.info(error);
            }
        );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.orderentity.deletebyfilter',
        [
            'fields' => [
                'orderId' => 5125,
                'ownerId' => 6933,
                'ownerTypeId' => 2
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1719325693.109545,
        "finish": 1719325695.863527,
        "duration": 2.7539820671081543,
        "processing": 1.773292064666748,
        "date_start": "2024-06-25T16:28:13+02:00",
        "date_finish": "2024-06-25T16:28:15+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Indicates whether the operation was successful ||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "0",
    "error_description": "Required fields: ownerId, orderId"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Errors

#|  
|| **Code** | **Description** ||
|| `200040300020` | `Access Denied` 
Insufficient access permissions
||
|| `201640400004` | `entity relation is not exists` 
Order binding to the CRM object not found
||
|| `200540400001` | `order does not exist` 
Order not found
||
|| `0` | `Required fields: #FIELDS#` 
Required fields not specified (`#FIELDS#` — list of fields separated by commas)
||
|| `0` | Various order saving errors
||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-order-entity-add.md)
- [{#T}](./crm-order-entity-list.md)
- [{#T}](./crm-order-entity-get-fields.md)