# Delete Delivery Service sale.delivery.delete

> Scope: [`sale`](../../../scopes/permissions.md)
>
> Who can execute the method: CRM administrator

This method deletes a delivery service.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ID***
[`sale_delivery_service.ID`](../../data-types.md) | Identifier of the delivery service   ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ID":196}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.delivery.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ID":196,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.delivery.delete
    ```

- JS

    ```js
    BX24.callMethod(
        'sale.delivery.delete', {
            ID: 196,
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
        'sale.delivery.delete',
        [
            'ID' => 196
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
   "result":true,
   "time":{
      "start":1714133231.200359,
      "finish":1714133231.954504,
      "duration":0.7541449069976807,
      "processing":0.4400501251220703,
      "date_start":"2024-04-26T15:07:11+03:00",
      "date_finish":"2024-04-26T15:07:11+03:00"
   }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Result of the delivery service deletion ||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**, **403**

```json
{
   "error":"ERROR_DELIVERY_NOT_FOUND",
   "error_description":"Delivery not found"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Status** ||
|| `ERROR_DELIVERY_NOT_FOUND` | Delivery service with the specified identifier (ID) not found | 400 ||
|| `ERROR_CHECK_FAILURE` | Validation error of incoming parameters (details in the error description) | 400 ||
|| `ERROR_DELIVERY_DELETE` | Error when attempting to delete the delivery service | 400 ||
|| `ACCESS_DENIED` | Insufficient permissions to delete the delivery service | 403 ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sale-delivery-add.md)
- [{#T}](./sale-delivery-update.md)
- [{#T}](./sale-delivery-config-update.md)
- [{#T}](./sale-delivery-config-get.md)
- [{#T}](./sale-delivery-get-list.md)