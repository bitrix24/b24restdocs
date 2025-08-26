# Get Delivery Information by ID crm.item.delivery.get

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: read access permission for the delivery order is required

This method retrieves brief information about the delivery.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`sale_order_shipment.id`](../../../sale/data-types.md#sale_order_shipment) | Delivery identifier ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":4077}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.delivery.get
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":4077,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.item.delivery.get
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.item.delivery.get', {
    			id: 4077,
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

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.item.delivery.get',
                [
                    'id' => 4077,
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting delivery item: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.item.delivery.get', {
            id: 4077,
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.item.delivery.get',
        [
            'id' => 4077
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Successful Response

HTTP Status: **200**

```json
{
   "result": {
      "id": 4077,
      "accountNumber": "3657/2",
      "priceDelivery": 79.99,
      "currency": "USD",
      "deducted": "N",
      "dateDeducted": null,
      "deliveryId": 228,
      "deliveryName": "Uber Taxi (Cargo)"
   },
   "time": {
      "start": 1716369295.614557,
      "finish": 1716369296.143089,
      "duration": 0.5285320281982422,
      "processing": 0.2371680736541748,
      "date_start": "2024-05-22T12:14:55+02:00",
      "date_finish": "2024-05-22T12:14:56+02:00"
   }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`sale_order_shipment_crm_simple`](#sale_order_shipment_crm_simple) | Object containing brief information about the delivery ||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

### Key result. Object of type 
### sale_order_shipment_crm_simple 

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`sale_order_shipment.id`](../../../data-types.md#sale_order_shipment) | Delivery identifier ||
|| **accountNumber**
[`string`](../../../data-types.md) | System delivery number ||
|| **deducted**
[`string`](../../../data-types.md) | Indicates whether the delivery has been shipped.
Possible values:
- `Y` — yes (shipped)
- `N` — no (not shipped)
 ||
|| **dateDeducted**
[`datetime`](../../../data-types.md)  | Date of the shipped flag change ||
|| **priceDelivery**
[`double`](../../../data-types.md)  | Delivery cost ||
|| **currency**
[`string`](../../../data-types.md)  | Delivery currency ||
|| **deliveryId**
[`sale_delivery_service.ID`](../../../data-types.md#sale_delivery_service)  | Delivery service identifier ||
|| **deliveryName**
[`string`](../../../data-types.md)  | Delivery service name ||
|#

## Error Handling

HTTP Status: **400**

```json
{
   "error": 0,
   "error_description": "Insufficient permissions"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `0` | Payment not found or access denied ||
|| `100` | Parameter id not specified ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include notitle [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-item-delivery-list.md)