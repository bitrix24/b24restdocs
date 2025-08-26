# Add an item to the shipment table part sale.shipmentitem.add

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `sale.shipmentitem.add` adds an item to the shipment table part.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | Field values for creating an item in the shipment table part ||
|#

### Parameter fields

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **orderDeliveryId***
[`sale_order_shipment.id`](../data-types.md) | Shipment identifier ||
|| **basketId***
[`sale_basket_item.id`](../data-types.md) | Basket identifier ||
|| **quantity***
[`double`](../../data-types.md) | Quantity of the product ||
|| **xmlId**
[`string`](../../data-types.md) | External identifier.

Can be used to synchronize the current product position in the shipment with a similar position in an external system ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```curl
    -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"orderDeliveryId":33,"basketId":18,"quantity":1}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webbhook_here**/sale.shipmentitem.add
    ```

- cURL (OAuth)

    ```curl
    -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"orderDeliveryId":33,"basketId":18,"quantity":1},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.shipmentitem.add
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'sale.shipmentitem.add',
    		{
    			fields: {
    				orderDeliveryId: 33,
    				basketId: 18,
    				quantity: 1
    			}
    		}
    	);
    	
    	const result = response.getData().result;
    	console.log(result);
    }
    catch( error )
    {
    	console.error(error.ex);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'sale.shipmentitem.add',
                [
                    'fields' => [
                        'orderDeliveryId' => 33,
                        'basketId'        => 18,
                        'quantity'        => 1,
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error()->ex);
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding shipment item: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'sale.shipmentitem.add',
        {
            fields: {
                orderDeliveryId: 33,
                basketId: 18,
                quantity: 1
            }
        },
        function(result)
        {
            if(result.error())
                console.error(result.error().ex);
            else
                console.log(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sale.shipmentitem.add',
        [
            'fields' => [
                'orderDeliveryId' => 33,
                'basketId' => 18,
                'quantity' => 1
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
        "shipmentItem": {
            "basketId": 2716,
            "dateInsert": "2024-04-11T09:10:34+02:00",
            "id": 7,
            "orderDeliveryId": 2431,
            "quantity": 3,
            "reservedQuantity": 0,
            "xmlId": "myXmlId"
        }
    },
    "time": {
        "start": 1712819431.708122,
        "finish": 1712819435.985352,
        "duration": 4.2772300243377686,
        "processing": 4.085968971252441,
        "date_start": "2024-04-11T10:10:31+02:00",
        "date_finish": "2024-04-11T10:10:35+02:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **shipmentItem**
[`sale_order_shipment_item`](../data-types.md) | Object with information about the added item in the shipment table part ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": 201250000001,
    "error_description": "Duplicate entry for key [basketId, orderDeliveryId]"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `201250000001` | An item with the specified field values `basketId` and `orderDeliveryId` already exists.

To change the quantity of the product, use the method [`sale.shipmentitem.update`](./sale-shipment-item-update.md) ||
|| `201240400002` | Shipment not found. Invalid value for the passed parameter `orderDeliveryId` ||
|| `201240400003` | Basket not found. Invalid value for the passed parameter `basketId` ||
|| `200040300020` | Insufficient permissions to add an item to the shipment table part ||
|| `100` | The `fields` parameter is missing or empty ||
|| `0` | Required fields are not provided ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sale-shipment-item-update.md)
- [{#T}](./sale-shipment-item-get.md)
- [{#T}](./sale-shipment-item-list.md)
- [{#T}](./sale-shipment-item-delete.md)
- [{#T}](./sale-shipment-item-get-fields.md)