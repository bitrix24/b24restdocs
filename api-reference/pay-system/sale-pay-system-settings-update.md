# Update Payment System Settings sale.paysystem.settings.update

> Scope: [`pay_system`](../scopes/permissions.md)
>
> Who can execute the method: CRM administrator (permission "Allow changing settings")

This method updates the payment system settings. The structure of the settings is defined when adding the payment system handler in the method [sale.paysystem.handler.add](./sale-pay-system-handler-add.md) under the `CODES` key of the `SETTINGS` parameter.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ID***
[`sale_paysystem.ID`](../sale/data-types.md) | Identifier of the payment system for which settings need to be retrieved
||
|| **PERSON_TYPE_ID**
[`sale_person_type.id`](../sale/data-types.md) | Identifier of the payer type for which settings need to be retrieved
||
|| **SETTINGS***
[`object`](../data-types.md) | Settings to be updated. The keys are the names of the settings, and the values are objects whose structure is described [below](#parametr-settings)
||
|#

### SETTINGS Parameter

#|
|| **Name**
`type` | **Description** ||
|| **TYPE**
[`string`](../data-types.md) | Source of the parameter value ||
|| **VALUE**
[`string`](../data-types.md) | Parameter code at the source or parameter value (for `TYPE="VALUE"`) ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ID":11,"PERSON_TYPE_ID":1,"SETTINGS":{"REST_SERVICE_KEY_IFRAME":{"TYPE":"VALUE","VALUE":"NEW_KEY"}}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.paysystem.settings.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ID":11,"PERSON_TYPE_ID":1,"SETTINGS":{"REST_SERVICE_KEY_IFRAME":{"TYPE":"VALUE","VALUE":"NEW_KEY"}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.paysystem.settings.update
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'sale.paysystem.settings.update',
    		{
    			'ID': 11,
    			'PERSON_TYPE_ID': 1,
    			'SETTINGS': {
    				'REST_SERVICE_KEY_IFRAME': {
    					'TYPE': 'VALUE',
    					'VALUE': 'NEW_KEY',
    				}
    			}
    		}
    	);
    	
    	const result = response.getData().result;
    	console.dir(result);
    }
    catch( error )
    {
    	console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'sale.paysystem.settings.update',
                [
                    'ID'            => 11,
                    'PERSON_TYPE_ID' => 1,
                    'SETTINGS'      => [
                        'REST_SERVICE_KEY_IFRAME' => [
                            'TYPE'  => 'VALUE',
                            'VALUE' => 'NEW_KEY',
                        ],
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your required data processing logic
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating payment system settings: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod('sale.paysystem.settings.update', {
        'ID': 11,
        'PERSON_TYPE_ID': 1,
        'SETTINGS': {
            'REST_SERVICE_KEY_IFRAME': {
                'TYPE': 'VALUE',
                'VALUE': 'NEW_KEY',
            }
        }
    }, 
    function(result) 
    { 
        if(result.error()) 
            console.error(result.error()); 
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
        'sale.paysystem.settings.update',
        [
            'ID' => 11,
            'PERSON_TYPE_ID' => 1,
            'SETTINGS' => [
                'REST_SERVICE_KEY_IFRAME' => [
                    'TYPE' => 'VALUE',
                    'VALUE' => 'NEW_KEY',
                ]
            ]
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
[`boolean`](../data-types.md) | Result of updating the payment system settings ||
|| **time**
[`time`](../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**, **403**

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
|| `ACCESS_DENIED` | Insufficient rights to read settings | 403 ||
|| `ERROR_CHECK_FAILURE` | One of the required fields is missing or the specified payment system was not found (details in the error description) | 400 ||
|| `ERROR_HANDLER_NOT_FOUND` | The `SETTINGS` field is missing or an empty object was passed | 400 ||
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
- [{#T}](./sale-pay-system-delete.md)
- [{#T}](./sale-pay-system-pay-payment.md)
- [{#T}](./sale-pay-system-pay-invoice.md)
- [{#T}](./sale-pay-system-settings-payment-get.md)
- [{#T}](./sale-pay-system-settings-invoice-get.md)