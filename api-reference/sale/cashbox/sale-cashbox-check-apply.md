# Save the result of printing the receipt sale.cashbox.check.apply

> Scope: [`sale, cashbox`](../../scopes/permissions.md)
>
> Who can execute the method: CRM administrator (permission "Allow to change settings")

The method saves the result of printing a receipt that was printed on a REST cash register. The UUID of the receipt is saved when it is printed from the `PRINT_URL` response specified when adding the handler (see [example of implementing a simple cash register on REST API](../../../tutorials/sale/cashbox-add-example.md)).

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **UUID***
[`string`](../../data-types.md) | UUID of the receipt ||
|| **PRINT_END_TIME**
[`string`](../../data-types.md) | Time when the receipt printing ended ||
|| **REG_NUMBER_KKT**
[`string`](../../data-types.md) | Registration number of the cash register ||
|| **FISCAL_DOC_ATTR**
[`string`](../../data-types.md) | Fiscal attribute of the document ||
|| **FISCAL_DOC_NUMBER**
[`string`](../../data-types.md) | Fiscal document number ||
|| **FISCAL_RECEIPT_NUMBER**
[`string`](../../data-types.md) | Fiscal receipt number ||
|| **FN_NUMBER**
[`string`](../../data-types.md) | Fiscal storage number ||
|| **SHIFT_NUMBER**
[`string`](../../data-types.md) | Shift number ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"UUID":"check|example.com|1","PRINT_END_TIME":"1609459200","REG_NUMBER_KKT":"1234567891011121","FISCAL_DOC_ATTR":"1234567890","FISCAL_DOC_NUMBER":"12345","FISCAL_RECEIPT_NUMBER":"123","FN_NUMBER":"1234567891011121","SHIFT_NUMBER":"1"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webbhook_here**/sale.cashbox.check.apply
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"UUID":"check|example.com|1","PRINT_END_TIME":"1609459200","REG_NUMBER_KKT":"1234567891011121","FISCAL_DOC_ATTR":"1234567890","FISCAL_DOC_NUMBER":"12345","FISCAL_RECEIPT_NUMBER":"123","FN_NUMBER":"1234567891011121","SHIFT_NUMBER":"1","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.cashbox.check.apply
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"sale.cashbox.check.apply",
    		{
    			'UUID':'check|example.com|1',
    			'PRINT_END_TIME':'1609459200',
    			'REG_NUMBER_KKT':'1234567891011121',
    			'FISCAL_DOC_ATTR':'1234567890',
    			'FISCAL_DOC_NUMBER':'12345',
    			'FISCAL_RECEIPT_NUMBER':'123',
    			'FN_NUMBER':'1234567891011121',
    			'SHIFT_NUMBER':'1'
    		}
    	);
    	
    	const result = response.getData().result;
    	if(result.error())
    		console.error(result.error());
    	else
    		console.dir(result);
    }
    catch(error)
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
                'sale.cashbox.check.apply',
                [
                    'UUID'                => 'check|example.com|1',
                    'PRINT_END_TIME'      => '1609459200',
                    'REG_NUMBER_KKT'      => '1234567891011121',
                    'FISCAL_DOC_ATTR'     => '1234567890',
                    'FISCAL_DOC_NUMBER'   => '12345',
                    'FISCAL_RECEIPT_NUMBER' => '123',
                    'FN_NUMBER'           => '1234567891011121',
                    'SHIFT_NUMBER'        => '1',
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Success: ' . print_r($result->data(), true);
            // Your data processing logic
            processData($result->data());
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error applying cash register check: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "sale.cashbox.check.apply",
        {
            'UUID':'check|example.com|1',
            'PRINT_END_TIME':'1609459200',
            'REG_NUMBER_KKT':'1234567891011121',
            'FISCAL_DOC_ATTR':'1234567890',
            'FISCAL_DOC_NUMBER':'12345',
            'FISCAL_RECEIPT_NUMBER':'123',
            'FN_NUMBER':'1234567891011121',
            'SHIFT_NUMBER':'1'
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sale.cashbox.check.apply',
        [
            'UUID' => 'check|example.com|1',
            'PRINT_END_TIME' => '1609459200',
            'REG_NUMBER_KKT' => '1234567891011121',
            'FISCAL_DOC_ATTR' => '1234567890',
            'FISCAL_DOC_NUMBER' => '12345',
            'FISCAL_RECEIPT_NUMBER' => '123',
            'FN_NUMBER' => '1234567891011121',
            'SHIFT_NUMBER' => '1'
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
[`boolean`](../../data-types.md) | Result of saving the receipt printing ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**, **403**

```json
{ 
    "error": "ERROR_CHECK_NOT_FOUND", 
    "error_description": "Check not found" 
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Status** ||
|| `ACCESS_DENIED` | Insufficient permissions to save the receipt printing result | 403 ||
|| `ERROR_CHECK_FAILURE` | The `UUID` field value is not specified | 400 ||
|| `ERROR_CHECK_NOT_FOUND` | Receipt with the specified `UUID` not found | 400 ||
|| `ERROR_CHECK_APPLY` | Other errors. More detailed information about the error can be found in `error_description` | 400 ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sale-cashbox-handler-add.md)
- [{#T}](./sale-cashbox-handler-update.md)
- [{#T}](./sale-cashbox-handler-list.md)
- [{#T}](./sale-cashbox-handler-delete.md)
- [{#T}](./sale-cashbox-add.md)
- [{#T}](./sale-cashbox-update.md)
- [{#T}](./sale-cashbox-list.md)
- [{#T}](./sale-cashbox-delete.md)