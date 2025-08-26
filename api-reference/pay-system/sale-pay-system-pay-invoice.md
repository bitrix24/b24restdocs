# Pay an invoice through a specific payment system sale.paysystem.pay.invoice

> Scope: [`pay_system`](../scopes/permissions.md)
>
> Who can execute the method: a user with permissions to create and edit CRM invoices (old version)

This method is used to pay an invoice (old version) through a specific payment system. It is called after processing the response from the payment system.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **INVOICE_ID***
[`integer`](../data-types.md) | Identifier of the old version invoice. To retrieve information about invoices, use the service [crm.invoice.*](../crm/outdated/invoice/index.md)
||
|| **PAY_SYSTEM_ID**
[`sale_paysystem.ID`](../sale/data-types.md) | Identifier of the payment system
||
|| **BX_REST_HANDLER**
[`string`](../data-types.md) | Symbolic identifier of the payment system's REST handler.

You must pass either the `PAY_SYSTEM_ID` parameter or the `BX_REST_HANDLER`:
- when passing `PAY_SYSTEM_ID`, the payment system with the specified identifier is used 
- when passing `BX_REST_HANDLER`, the first found payment system with the specified handler is used 

If both parameters are passed, the `PAY_SYSTEM_ID` parameter takes precedence.
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
    -d '{"INVOICE_ID":2,"PAY_SYSTEM_ID":31}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.paysystem.pay.invoice
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"INVOICE_ID":2,"PAY_SYSTEM_ID":31,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.paysystem.pay.invoice
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'sale.paysystem.pay.invoice',
    		{
    			"INVOICE_ID": 2,
    			"PAY_SYSTEM_ID": 31,
    		}
    	);
    	
    	const result = response.getData().result;
    	console.dir(result);
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
                'sale.paysystem.pay.invoice',
                [
                    'INVOICE_ID'    => 2,
                    'PAY_SYSTEM_ID' => 31,
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your logic for processing data
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error paying invoice: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod('sale.paysystem.pay.invoice', {
        "INVOICE_ID": 2,
        "PAY_SYSTEM_ID": 31,
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

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sale.paysystem.pay.invoice',
        [
            'INVOICE_ID' => 2,
            'PAY_SYSTEM_ID' => 31
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
[`boolean`](../data-types.md) | Result of the invoice payment ||
|| **time**
[`time`](../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": " ERROR_CHECK_FAILURE",
    "error_description": "Pay system not found"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Status** ||
|| `ACCESS_DENIED` | Insufficient permissions to pay the invoice | 403 ||
|| `ERROR_CHECK_FAILURE` | One of the required fields is missing, the payment system with the specified REST handler was not found, or the invoice with the specified `ID` was not found (details can be found in the error description) | 400 ||
|| `ERROR_PAY_INVOICE_NOT_SUPPORTED` | Invoice payments are not supported on the account | 400 ||
|| `ERROR_INTERNAL_INVOICE_NOT_FOUND` | Invoice with the specified `ID` was not found | 400 ||
|| `ERROR_PROCESS_REQUEST_RESULT` | Payment system with the specified `ID` was not found or an error occurred while processing the request by the payment system (details can be found in the error description) | 400 ||
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
- [{#T}](./sale-pay-system-pay-payment.md)
- [{#T}](./sale-pay-system-settings-payment-get.md)
- [{#T}](./sale-pay-system-settings-invoice-get.md)