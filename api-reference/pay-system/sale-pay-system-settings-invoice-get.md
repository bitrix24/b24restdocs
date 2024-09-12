# Get Payment System Settings for a Specific Invoice sale.paysystem.settings.invoice.get

> Scope: [`pay_system`](../scopes/permissions.md)
>
> Who can execute the method: a user with permissions to create and edit CRM invoices (legacy version)

This method returns the payment system settings for a specific invoice (legacy version).

## Method Parameters

{% include [Note on Required Parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **INVOICE_ID***
[`integer`](../data-types.md) | Identifier of the legacy invoice. To retrieve information about invoices, use the service [crm.invoice.*](../crm/outdated/invoice/index.md)
||
|| **PAY_SYSTEM_ID**
[`sale_paysystem.ID`](../sale/data-types.md) | Identifier of the payment system
||
|| **BX_REST_HANDLER**
[`sale_paysystem.ACTION_FILE`](../sale/data-types.md) | Symbolic identifier of the payment system REST handler
||
|#

You must provide either the `PAY_SYSTEM_ID` parameter or the `BX_REST_HANDLER`:
- When providing `PAY_SYSTEM_ID`, the payment system with the specified identifier is used.
- When providing `BX_REST_HANDLER`, the first found payment system with the specified handler is used.

If both parameters are provided, the `PAY_SYSTEM_ID` parameter takes precedence.

## Code Examples

{% include [Note on Examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"INVOICE_ID":10,"PAY_SYSTEM_ID":11}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.paysystem.settings.invoice.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"INVOICE_ID":10,"PAY_SYSTEM_ID":11,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.paysystem.settings.invoice.get
    ```

- JS

    ```js
    BX24.callMethod('sale.paysystem.settings.invoice.get', {
            "INVOICE_ID": 10,
            "PAY_SYSTEM_ID": 11
        }, 
        function(result) 
        { 
            if(result.error()) 
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
        'sale.paysystem.settings.invoice.get',
        [
            'INVOICE_ID' => 10,
            'PAY_SYSTEM_ID' => 11
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
    "result": {
        "REST_SERVICE_ID_IFRAME": "snum",
        "REST_SERVICE_KEY_IFRAME": "skey",
        "PS_WORK_MODE_IFRAME": "REGULAR"
    },
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
[`object`](../data-types.md) | Root element of the response. 

The keys of the object are the parameter codes specified when adding the handler via [sale.paysystem.handler.add](./sale-pay-system-handler-add.md) in the `CODES` parameter. 

The values of the object are the parameter values:
- either filled in manually by the user when creating the payment system
- or specified when adding the payment system via [sale.paysystem.add](./sale-pay-system-add.md)
||
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
|| `ACCESS_DENIED` | Insufficient permissions to retrieve settings | 403 ||
|| `ERROR_CHECK_FAILURE` | One of the required fields is missing or the payment system with the specified `ID` or `bx_rest_handler` was not found (details in the error description) | 400 ||
|| `ERROR_INTERNAL_INVOICE_NOT_FOUND` | The specified invoice was not found | 400 ||
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
- [{#T}](./sale-pay-system-settings-get.md)
- [{#T}](./sale-pay-system-settings-update.md)
- [{#T}](./sale-pay-system-delete.md)
- [{#T}](./sale-pay-system-pay-payment.md)
- [{#T}](./sale-pay-system-pay-invoice.md)
- [{#T}](./sale-pay-system-settings-payment-get.md)