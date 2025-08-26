# Get Available Shipment Fields sale.shipment.getfields

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method retrieves the available shipment fields.

No parameters.

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.shipment.getfields
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.shipment.getfields
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"sale.shipment.getfields", {}
    	);
    	
    	const result = response.getData().result;
    	console.info(result);
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
                'sale.shipment.getfields',
                []
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            echo 'Data: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting shipment fields: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "sale.shipment.getfields", {},
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.info(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sale.shipment.getfields',
        []
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Successful Response

HTTP status: **200**

```json
{
   "result":{
      "shipment":{
         "accountNumber":{
            "isImmutable":false,
            "isReadOnly":true,
            "isRequired":false,
            "type":"string"
         },
         "allowDelivery":{
            "isImmutable":false,
            "isReadOnly":false,
            "isRequired":true,
            "type":"char"
         },
         "basePriceDelivery":{
            "isImmutable":false,
            "isReadOnly":false,
            "isRequired":false,
            "type":"double"
         },
         "canceled":{
            "isImmutable":false,
            "isReadOnly":true,
            "isRequired":false,
            "type":"char"
         },
         "comments":{
            "isImmutable":false,
            "isReadOnly":false,
            "isRequired":false,
            "type":"string"
         },
         "companyId":{
            "isImmutable":false,
            "isReadOnly":false,
            "isRequired":false,
            "type":"integer"
         },
         "currency":{
            "isImmutable":false,
            "isReadOnly":true,
            "isRequired":false,
            "type":"string"
         },
         "customPriceDelivery":{
            "isImmutable":false,
            "isReadOnly":true,
            "isRequired":false,
            "type":"char"
         },
         "dateAllowDelivery":{
            "isImmutable":false,
            "isReadOnly":true,
            "isRequired":false,
            "type":"datetime"
         },
         "dateCanceled":{
            "isImmutable":false,
            "isReadOnly":true,
            "isRequired":false,
            "type":"datetime"
         },
         "dateDeducted":{
            "isImmutable":false,
            "isReadOnly":true,
            "isRequired":false,
            "type":"datetime"
         },
         "dateInsert":{
            "isImmutable":false,
            "isReadOnly":true,
            "isRequired":false,
            "type":"datetime"
         },
         "dateMarked":{
            "isImmutable":false,
            "isReadOnly":true,
            "isRequired":false,
            "type":"datetime"
         },
         "dateResponsibleId":{
            "isImmutable":false,
            "isReadOnly":true,
            "isRequired":false,
            "type":"datetime"
         },
         "deducted":{
            "isImmutable":false,
            "isReadOnly":false,
            "isRequired":true,
            "type":"char"
         },
         "deliveryDocDate":{
            "isImmutable":false,
            "isReadOnly":false,
            "isRequired":false,
            "type":"datetime"
         },
         "deliveryDocNum":{
            "isImmutable":false,
            "isReadOnly":false,
            "isRequired":false,
            "type":"string"
         },
         "deliveryId":{
            "isImmutable":false,
            "isReadOnly":false,
            "isRequired":true,
            "type":"integer"
         },
         "deliveryName":{
            "isImmutable":false,
            "isReadOnly":true,
            "isRequired":false,
            "type":"string"
         },
         "deliveryXmlId":{
            "isImmutable":false,
            "isReadOnly":true,
            "isRequired":false,
            "type":"string"
         },
         "discountPrice":{
            "isImmutable":false,
            "isReadOnly":true,
            "isRequired":false,
            "type":"double"
         },
         "empAllowDeliveryId":{
            "isImmutable":false,
            "isReadOnly":true,
            "isRequired":false,
            "type":"integer"
         },
         "empCanceledId":{
            "isImmutable":false,
            "isReadOnly":true,
            "isRequired":false,
            "type":"integer"
         },
         "empDeductedId":{
            "isImmutable":false,
            "isReadOnly":true,
            "isRequired":false,
            "type":"integer"
         },
         "empMarkedId":{
            "isImmutable":false,
            "isReadOnly":true,
            "isRequired":false,
            "type":"integer"
         },
         "empResponsibleId":{
            "isImmutable":false,
            "isReadOnly":true,
            "isRequired":false,
            "type":"integer"
         },
         "externalDelivery":{
            "isImmutable":false,
            "isReadOnly":true,
            "isRequired":false,
            "type":"char"
         },
         "id":{
            "isImmutable":false,
            "isReadOnly":true,
            "isRequired":false,
            "type":"integer"
         },
         "id1c":{
            "isImmutable":false,
            "isReadOnly":true,
            "isRequired":false,
            "type":"string"
         },
         "marked":{
            "isImmutable":false,
            "isReadOnly":true,
            "isRequired":false,
            "type":"char"
         },
         "orderId":{
            "isImmutable":true,
            "isReadOnly":false,
            "isRequired":true,
            "type":"integer"
         },
         "priceDelivery":{
            "isImmutable":false,
            "isReadOnly":false,
            "isRequired":false,
            "type":"double"
         },
         "reasonMarked":{
            "isImmutable":false,
            "isReadOnly":true,
            "isRequired":false,
            "type":"string"
         },
         "reasonUndoDeducted":{
            "isImmutable":false,
            "isReadOnly":true,
            "isRequired":false,
            "type":"string"
         },
         "responsibleId":{
            "isImmutable":false,
            "isReadOnly":false,
            "isRequired":false,
            "type":"integer"
         },
         "statusId":{
            "isImmutable":false,
            "isReadOnly":false,
            "isRequired":false,
            "type":"char"
         },
         "statusXmlId":{
            "isImmutable":false,
            "isReadOnly":true,
            "isRequired":false,
            "type":"char"
         },
         "system":{
            "isImmutable":false,
            "isReadOnly":true,
            "isRequired":false,
            "type":"char"
         },
         "trackingDescription":{
            "isImmutable":false,
            "isReadOnly":true,
            "isRequired":false,
            "type":"string"
         },
         "trackingLastCheck":{
            "isImmutable":false,
            "isReadOnly":true,
            "isRequired":false,
            "type":"string"
         },
         "trackingNumber":{
            "isImmutable":false,
            "isReadOnly":false,
            "isRequired":false,
            "type":"string"
         },
         "trackingStatus":{
            "isImmutable":false,
            "isReadOnly":true,
            "isRequired":false,
            "type":"string"
         },
         "updated1c":{
            "isImmutable":false,
            "isReadOnly":true,
            "isRequired":false,
            "type":"char"
         },
         "version1c":{
            "isImmutable":false,
            "isReadOnly":true,
            "isRequired":false,
            "type":"string"
         },
         "xmlId":{
            "isImmutable":false,
            "isReadOnly":false,
            "isRequired":false,
            "type":"string"
         }
      }
   },
   "time":{
      "start":1713190893.141474,
      "finish":1713190895.854316,
      "duration":2.7128419876098633,
      "processing":0.16173791885375977,
      "date_start":"2024-04-15T17:21:33+02:00",
      "date_finish":"2024-04-15T17:21:35+02:00"
   }
}
```
### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **shipment**
[`object`](../../data-types.md) | Object in the format `{"field_1": "value_1", ... "field_N": "value_N"}`, where `field` is the identifier of the object [`sale_order_shipment`](../data-types.md#sale_order_shipment), and `value` is an object of type [`rest_field_description`](../data-types.md#rest_field_description) ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
   "error":0,
   "error_description":"error"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300010` | Insufficient permissions to read available shipment fields ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include notitle [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./sale-shipment-add.md)
- [{#T}](./sale-shipment-update.md)
- [{#T}](./sale-shipment-get.md)
- [{#T}](./sale-shipment-list.md)
- [{#T}](./sale-shipment-delete.md)