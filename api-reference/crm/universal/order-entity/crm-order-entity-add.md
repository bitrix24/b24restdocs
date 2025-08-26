# Add Order Binding to CRM Object crm.orderentity.add

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: online store administrator

This method adds a binding of an order to a CRM object.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../../data-types.md) | Field values for creating the binding ||
|#

### Parameter fields

{% include [Note on required parameters](../../../../_includes/required.md) %}

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

For invoices, it can be obtained using the [crm.invoice.list](../../outdated/invoice/crm-invoice-list.md)
||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

Add order binding to a deal:

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"orderId":5125,"ownerId":6933,"ownerTypeId":2}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.orderentity.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"orderId":5125,"ownerId":6933,"ownerTypeId":2},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.orderentity.add
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"crm.orderentity.add",
    		{
    			fields: {
    				orderId: 5125,
    				ownerId: 6933,
    				ownerTypeId: 2
    			}
    		}
    	);
    	
    	const result = response.getData().result;
    	console.log(result);
    }
    catch( error )
    {
    	console.error(error);
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.orderentity.add",
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

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.orderentity.add',
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

HTTP status: **200**

```json
{
    "result": {
        "dealOrder": {
            "orderId": 5125,
            "ownerId": 6933,
            "ownerTypeId": 2
        }
    },
    "time": {
        "start": 1719325851.568441,
        "finish": 1719325852.546479,
        "duration": 0.9780380725860596,
        "processing": 0.32780981063842773,
        "date_start": "2024-06-25T16:30:51+02:00",
        "date_finish": "2024-06-25T16:30:52+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Root element of the response ||
|| **dealOrder**
[`crm_orderentity`](../../data-types.md#crm_orderentity) | Object with information about the created binding ||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "201650000001",
    "error_description": "Duplicate entry for key [ownerId, ownerTypeId, orderId]"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Errors

#|  
|| **Code** | **Description** ||
|| `200040300020` | `Access Denied` 
Insufficient access permissions
||
|| `201650000001` | `Duplicate entry for key [ownerId, ownerTypeId, orderId]` 
Binding already exists
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

- [{#T}](./crm-order-entity-list.md)
- [{#T}](./crm-order-entity-delete-by-filter.md)
- [{#T}](./crm-order-entity-get-fields.md)