# Change setting for recurring invoice crm.invoice.recurring.update

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning %}

The method is deprecated. It is recommended to use [`Universal methods for invoices`](../../universal/invoice.md)

{% endnote %}

The method updates an existing setting for the recurring invoice template.

## Method parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Identifier of the recurring invoice template setting ||
|| **fields**
[`array`](../../../data-types.md) | Field values for updating the setting.

To find out the required format of the fields, execute the method [crm.invoice.recurring.fields](./crm-invoice-recurring-fields.md) and check the format of the returned values for these fields ||
|#

## Code examples

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":"your_recurring_invoice_id","fields":{"SEND_BILL":"Y","EMAIL_ID":136,"PARAMS":{"MODE":"month","TYPE":2,"INTERVAL":3,"WEEKDAY":"Monday","NUM_WEEKDAY_IN_MONTH":4,"DATE_PAY_BEFORE_OFFSET_TYPE":"day","DATE_PAY_BEFORE_OFFSET_VALUE":15}}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.invoice.recurring.update
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":"your_recurring_invoice_id","fields":{"SEND_BILL":"Y","EMAIL_ID":136,"PARAMS":{"MODE":"month","TYPE":2,"INTERVAL":3,"WEEKDAY":"Monday","NUM_WEEKDAY_IN_MONTH":4,"DATE_PAY_BEFORE_OFFSET_TYPE":"day","DATE_PAY_BEFORE_OFFSET_VALUE":15}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.invoice.recurring.update
    ```

- JS

    ```js
    try
    {
    	const id = prompt("Enter ID");
    	const response = await $b24.callMethod(
    		"crm.invoice.recurring.update",
    		{
    			id: id,
    			fields:
    			{
    				"SEND_BILL": "Y",
    				"EMAIL_ID": 136,
    				"PARAMS": {
    					"MODE": "month",
    					"TYPE": 2,
    					"INTERVAL": 3,
    					"WEEKDAY": "Monday",
    					"NUM_WEEKDAY_IN_MONTH": 4,
    					"DATE_PAY_BEFORE_OFFSET_TYPE": "day",
    					"DATE_PAY_BEFORE_OFFSET_VALUE": 15,
    				}
    			},
    		}
    	);
    	
    	const result = response.getData().result;
    	if(result.error())
    		console.error(result.error());
    	else
    	{
    		console.info(result);
    	}
    }
    catch(error)
    {
    	console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $id = $_POST['id'];
    
        $response = $b24Service
            ->core
            ->call(
                'crm.invoice.recurring.update',
                [
                    'id' => $id,
                    'fields' => [
                        'SEND_BILL' => 'Y',
                        'EMAIL_ID' => 136,
                        'PARAMS' => [
                            'MODE' => 'month',
                            'TYPE' => 2,
                            'INTERVAL' => 3,
                            'WEEKDAY' => 'Monday',
                            'NUM_WEEKDAY_IN_MONTH' => 4,
                            'DATE_PAY_BEFORE_OFFSET_TYPE' => 'day',
                            'DATE_PAY_BEFORE_OFFSET_VALUE' => 15,
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
        echo 'Error updating recurring invoice: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    var id = prompt("Enter ID");
    BX24.callMethod(
        "crm.invoice.recurring.update",
        {
            id: id,
            fields:
            {
                "SEND_BILL": "Y",
                "EMAIL_ID": 136,
                "PARAMS": {
                    "MODE": "month",
                    "TYPE": 2,
                    "INTERVAL": 3,
                    "WEEKDAY": "Monday",
                    "NUM_WEEKDAY_IN_MONTH": 4,
                    "DATE_PAY_BEFORE_OFFSET_TYPE": "day",
                    "DATE_PAY_BEFORE_OFFSET_VALUE": 15,
                }
            },
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
            {
                console.info(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $id = 'your_recurring_invoice_id'; // Replace 'your_recurring_invoice_id' with the actual recurring invoice ID

    $result = CRest::call(
        'crm.invoice.recurring.update',
        [
            'id' => $id,
            'fields' => [
                'SEND_BILL' => 'Y',
                'EMAIL_ID' => 136,
                'PARAMS' => [
                    'MODE' => 'month',
                    'TYPE' => 2,
                    'INTERVAL' => 3,
                    'WEEKDAY' => 'Monday',
                    'NUM_WEEKDAY_IN_MONTH' => 4,
                    'DATE_PAY_BEFORE_OFFSET_TYPE' => 'day',
                    'DATE_PAY_BEFORE_OFFSET_VALUE' => 15,
                ]
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}