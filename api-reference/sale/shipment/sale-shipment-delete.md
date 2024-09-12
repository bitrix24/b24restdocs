# Delete Shipment sale.shipment.delete

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method deletes a shipment.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`sale_order_shipment.id`](../data-types.md) | Identifier of the shipment ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":2461}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.shipment.delete
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":2461,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.shipment.delete
    ```

- JS

    ```js
    BX24.callMethod(
        "sale.shipment.delete", {
            "id": 2461
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
        'sale.shipment.delete',
        [
            'id' => 2461
        ]
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
   "result":true,
   "time":{
      "start":1712840164.173512,
      "finish":1712840165.403435,
      "duration":1.2299230098724365,
      "processing":1.053415060043335,
      "date_start":"2024-04-11T15:56:04+03:00",
      "date_finish":"2024-04-11T15:56:05+03:00"
   }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Result of the shipment deletion ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
   "error":"SALE_SHIPMENT_EXIST_SHIPPED",
   "error_description":"The shipment has already been shipped. Cancel the shipment"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `201140400001` | The shipment to be deleted was not found ||
|| `200040300020` | Insufficient permissions to delete the shipment ||
|| `SALE_SHIPMENT_EXIST_SHIPPED` | Attempt to delete a shipped shipment ||
|| `100` | The `id` parameter is missing ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include notitle [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./sale-shipment-add.md)
- [{#T}](./sale-shipment-get.md)
- [{#T}](./sale-shipment-list.md)
- [{#T}](./sale-shipment-update.md)
- [{#T}](./sale-shipment-get-fields.md)