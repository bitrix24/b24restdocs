# Get Payment Binding by Id sale.paymentItemShipment.get

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method retrieves the payment binding to the shipment by `ID`.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`sale_payment_item_shipment.id`](../data-types.md) | Identifier of the payment binding to the shipment ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1183}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.paymentitemshipment.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1183,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.paymentitemshipment.get
    ```

- JS

    ```js
    BX24.callMethod(
        "sale.paymentitemshipment.get",
        {
            "id": 1183
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
        'sale.paymentitemshipment.get',
        [
            'id' => 1183
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
    "result":{
        "paymentItemShipment":{
            "dateInsert":"2024-04-15T10:53:42+03:00",
            "id":1183,
            "paymentId":1025,
            "shipmentId":2471,
            "xmlId":"myXmlId"
        }
    },
    "time":{
        "start":1713171284.974848,
        "finish":1713171285.256904,
        "duration":0.2820558547973633,
        "processing":0.035675048828125,
        "date_start":"2024-04-15T11:54:44+03:00",
        "date_finish":"2024-04-15T11:54:45+03:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **paymentItemShipment**
[`sale_payment_item_shipment`](../data-types.md) | Information about the payment binding to the shipment ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":201240400001,
    "error_description":"payment item does not exist"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `201240400001` | Payment binding to the shipment not found ||
|| `200040300010` | Insufficient permissions to read payment binding data to the shipment ||
|| `100` | Parameter `id` not specified ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./sale-payment-item-shipment-add.md)
- [{#T}](./sale-payment-item-shipment-update.md)
- [{#T}](./sale-payment-item-shipment-list.md)
- [{#T}](./sale-payment-item-shipment-delete.md)
- [{#T}](./sale-payment-item-shipment-get-fields.md)