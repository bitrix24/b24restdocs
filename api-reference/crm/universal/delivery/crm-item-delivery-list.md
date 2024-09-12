# Get a List of Deliveries for a CRM Object crm.item.delivery.list

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: requires read access permission for the crm object from which deliveries are selected.

This method retrieves a list of deliveries for a specific crm object.

## Method Parameters

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **entityId***
[`integer`](../../../data-types.md) | Identifier of the crm object ||
|| **entityTypeId***
[`integer`](../../../data-types.md) | Identifier of the [`crm object type`](../../data-types.md#crm-object-type) ||
|| **filter**
[`object`](../../../data-types.md) | Additional filter for cases when you need to retrieve not all deliveries of the crm object, but based on a more specific filter. 
The format of the `filter` parameter corresponds to that described in the [`sale.shipment.list`](../../../sale/shipment/sale-shipment-list.md) method. ||
|| **order**
[`object`](../../../data-types.md) | The format of the `order` parameter corresponds to that described in the [`sale.shipment.list`](../../../sale/shipment/sale-shipment-list.md) method. ||

|#

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityId":13127,"entityTypeId":2,"filter":{"@id":[4077,4078]}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.delivery.list
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityId":13127,"entityTypeId":2,"filter":{"@id":[4077,4078]},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.item.delivery.list
    ```

- JS

    ```js
    BX24.callMethod(
        'crm.item.delivery.list', {
            entityId: 13127,
            entityTypeId: 2,
            filter: {
                "@id": [4077, 4078]
            },
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

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.item.delivery.list',
        [
            'entityId' => 13127,
            'entityTypeId' => 2,
            'filter' => [
                "@id" => [4077, 4078]
            ]
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
   "result":[
      {
         "id":4077,
         "accountNumber":"3657\/2",
         "deducted":"N",
         "dateDeducted":null,
         "deliveryId":228,
         "priceDelivery":79.99,
         "currency":"USD",
         "deliveryName":"Uber Taxi (Cargo)"
      },
      {
         "id":4078,
         "accountNumber":"3657\/3",
         "deducted":"N",
         "dateDeducted":null,
         "deliveryId":228,
         "priceDelivery":79.99,
         "currency":"USD",
         "deliveryName":"Uber Taxi (Cargo)"
      }
   ],
   "time":{
      "start":1716369036.246855,
      "finish":1716369036.734466,
      "duration":0.4876110553741455,
      "processing":0.18442106246948242,
      "date_start":"2024-05-22T12:10:36+03:00",
      "date_finish":"2024-05-22T12:10:36+03:00"
   }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`sale_order_shipment_crm_simple`](./crm-item-delivery-get.md#sale_order_shipment_crm_simple) | Array of objects containing brief information about the selected deliveries ||
|| **time**
[`time`](../../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
   "error":0,
   "error_description":"Insufficient permissions"
}
```

{% include notitle [Error Handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `0` | Access denied ||
|| `100` | Required fields not provided ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include notitle [System Errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-item-delivery-get.md)