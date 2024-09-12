# Pay for an Order through a Specific Payment System sale.paysystem.pay.payment

> Scope: [`pay_system`](../scopes/permissions.md)
>
> Who can execute the method: a user with permissions to create and edit orders in CRM

This method is used to pay for an order through a specific payment system. It is called after processing the response from the payment system.

To perform the payment, there must be a payment linked to the specified payment system in the system.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **PAYMENT_ID***
[`sale_order_payment.id`](../sale/data-types.md) | Payment identifier
||
|| **PAY_SYSTEM_ID***
[`sale_paysystem.ID`](../sale/data-types.md) | Payment system identifier
||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"PAYMENT_ID":1,"PAY_SYSTEM_ID":1}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.paysystem.pay.payment
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"PAYMENT_ID":1,"PAY_SYSTEM_ID":1,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.paysystem.pay.payment
    ```

- JS

    ```js
    BX24.callMethod('sale.paysystem.pay.payment', {
            "PAYMENT_ID": 1,
            "PAY_SYSTEM_ID": 1,
        }, 
        function(result) 
        { 
            if (result.error()) 
            {
                console.error(result.error()); 
            }
            else 
            { 
                console.dir(result.data()); 
            } 
        } 
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sale.paysystem.pay.payment',
        [
            'PAYMENT_ID' => 1,
            'PAY_SYSTEM_ID' => 1
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
        "start": 1712135335.026931,
        "finish": 1712135335.407762,
        "duration": 0.3808310031890869,
        "processing": 0.0336611270904541,
        "date_start": "2024-04-03T11:08:55+02:00",
        "date_finish": "2024-04-03T11:08:55+02:00",
        "operating_reset_at": 1705765533,
        "operating": 3.3076241016387939
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../data-types.md) | Payment result ||
|| **time**
[`time`](../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "ERROR_CHECK_FAILURE",
    "error_description": "Pay system not found"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Status** ||
|| `ACCESS_DENIED` | Insufficient permissions to change the payment status | 403 ||
|| `ERROR_CHECK_FAILURE` | One of the required fields is missing, the specified payment system was not found, or the payment with the specified `ID` and payment system was not found (details can be found in the error description) | 400 ||
|| `ERROR_PROCESS_REQUEST_RESULT` | Error processing the request by the payment system (details can be found in the error description) | 400 ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sale-pay-system-handler-add.md)
- [{#T}](./sale-pay-system-handler-update.md)
- [{#T}](./sale-pay-system-handler-list.md)
- [{#T}](./sale-pay-system-handler-delete.md)
- [{#T}](./sale-pay-system-add.md)
- [{#T}](./sale-pay-system-update.md)
- [{#T}](./sale-pay-system-list.md)
- [{#T}](./sale-pay-system-delete.md)
- [{#T}](./sale-pay-system-settings-get.md)
- [{#T}](./sale-pay-system-settings-update.md)
- [{#T}](./sale-pay-system-pay-invoice.md)
- [{#T}](./sale-pay-system-settings-payment-get.md)
- [{#T}](./sale-pay-system-settings-invoice-get.md)