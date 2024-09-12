# Delete Delivery Request sale.delivery.request.delete

> Scope: [`sale, delivery`](../../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method deletes a delivery request.

## Method Parameters

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **DELIVERY_ID***
[`sale_delivery_service.ID`](../../data-types.md) | Identifier of the delivery service to which the delivery request belongs.

You can obtain the identifiers `sale_delivery_service.ID` of delivery services using the method [sale.delivery.getlist](../delivery/sale-delivery-get-list.md)
||
|| **REQUEST_ID***
[`string`](../../../data-types.md) | Identifier of the delivery request.

The identifier is assigned by the external system in response to the webhook for creating a delivery order (more details in the webhook description [Creating a Delivery Order](../webhooks/create-delivery-request.md))
||
|#

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"DELIVERY_ID":225,"REQUEST_ID":"4757aca4931a4f029f49c0db4374d13d"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.delivery.request.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"DELIVERY_ID":225,"REQUEST_ID":"4757aca4931a4f029f49c0db4374d13d","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.delivery.request.delete
    ```

- JS

    ```js
    BX24.callMethod(
        'sale.delivery.request.delete', {
            DELIVERY_ID: 225,
            REQUEST_ID: "4757aca4931a4f029f49c0db4374d13d",
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.info(result.data());
            }
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sale.delivery.request.delete',
        [
            'DELIVERY_ID' => 225,
            'REQUEST_ID' => "4757aca4931a4f029f49c0db4374d13d"
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
    "result":true,
    "time":{
        "start":1714549724.272976,
        "finish":1714549724.479944,
        "duration":0.20696806907653809,
        "processing":0.02615499496459961,
        "date_start":"2024-05-01T10:48:44+03:00",
        "date_finish":"2024-05-01T10:48:44+03:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Result of the delivery request deletion ||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error":"DELIVERY_NOT_FOUND",
    "error_description":"Delivery service has not been found"
}
```

{% include notitle [Error Handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Status** ||
|| `DELIVERY_ID_NOT_SPECIFIED` | Delivery service identifier not specified | `400` || 
|| `DELIVERY_NOT_FOUND` | Delivery service not found | `400` || 
|| `REQUEST_ID_NOT_SPECIFIED` | Delivery request identifier not specified | `400` ||
|| `REQUEST_NOT_FOUND` | Delivery request not found | `400` ||
|| `DELETE_REQUEST_INTERNAL_ERROR` | Error while attempting to delete the delivery request | `400` ||
|| `ACCESS_DENIED` | Insufficient permissions to add the delivery service | `403` ||
|#

{% include [System Errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sale-delivery-request-update.md)
- [{#T}](./sale-delivery-request-send-message.md)