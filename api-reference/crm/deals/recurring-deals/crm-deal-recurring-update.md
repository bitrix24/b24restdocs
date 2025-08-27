# Update existing settings for the recurring deal template crm.deal.recurring.update

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not specified
- examples (in other languages) are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.deal.recurring.update` updates the existing settings for the recurring deal template.

#|
|| **Parameter** | **Description** ||
|| **id** | Identifier of the recurring deal template settings. ||
|| **fields** | Set of fields – an array of the form `array("field_to_update"=>"value"[, ...])`, where "field_to_update" can take values returned by the method [crm.deal.recurring.fields](./crm-deal-recurring-fields.md). 

{% note info %}

To find out the required format of the fields, execute the method [crm.deal.recurring.fields](./crm-deal-recurring-fields.md) and check the format of the returned values for those fields. 

{% endnote %}
||
|#

## Example

{% list tabs %}

- JS


    ```js
    try
    {
    	const current = new Date();
    	const nextYear = new Date();
    	nextYear.setYear(current.getFullYear() + 1);
    
    	const date2str = function(d)
    	{
    		return d.getFullYear() + '-' + paddatepart(1 + d.getMonth()) + '-' + paddatepart(d.getDate()) + 'T' + paddatepart(d.getHours()) + ':' + paddatepart(d.getMinutes()) + ':' + paddatepart(d.getSeconds()) + '+02:00';
    	};
    
    	const paddatepart = function(part)
    	{
    		return part >= 10 ? part.toString() : '0' + part.toString();
    	};
    
    	const id = prompt("Enter ID");
    
    	const response = await $b24.callMethod(
    		"crm.deal.recurring.update",
    		{
    			id: id,
    			fields:
    			{
    				"CATEGORY_ID": "2",
    				"START_DATE": date2str(nextYear),
    				"PARAMS": {
    					"MODE": "single",
    					"SINGLE_BEFORE_START_DATE_TYPE": "day",
    					"SINGLE_BEFORE_START_DATE_VALUE": 5,
    					"OFFSET_BEGINDATE_TYPE": "day",
    					"OFFSET_BEGINDATE_VALUE": 1,
    					"OFFSET_CLOSEDATE_TYPE": "month",
    					"OFFSET_CLOSEDATE_VALUE": 2,
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
    	console.error(error);
    }
    ```

- PHP


    ```php
    try {
        $current = new DateTime();
        $nextYear = new DateTime();
        $nextYear->setDate($current->format('Y') + 1);
    
        $date2str = function($d) {
            return $d->format('Y-m-d\TH:i:sP');
        };
    
        $id = readline("Enter ID");
    
        $response = $b24Service
            ->core
            ->call(
                'crm.deal.recurring.update',
                [
                    'id' => $id,
                    'fields' => [
                        'CATEGORY_ID' => '2',
                        'START_DATE' => $date2str($nextYear),
                        'PARAMS' => [
                            'MODE' => 'single',
                            'SINGLE_BEFORE_START_DATE_TYPE' => 'day',
                            'SINGLE_BEFORE_START_DATE_VALUE' => 5,
                            'OFFSET_BEGINDATE_TYPE' => 'day',
                            'OFFSET_BEGINDATE_VALUE' => 1,
                            'OFFSET_CLOSEDATE_TYPE' => 'month',
                            'OFFSET_CLOSEDATE_VALUE' => 2,
                        ],
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating recurring deal: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    var current = new Date();
    var nextYear = new Date();
    nextYear.setYear(current.getFullYear() + 1);
    var date2str = function(d)
    {
        return d.getFullYear() + '-' + paddatepart(1 + d.getMonth()) + '-' + paddatepart(d.getDate()) + 'T' + paddatepart(d.getHours()) + ':' + paddatepart(d.getMinutes()) + ':' + paddatepart(d.getSeconds()) + '+02:00';
    };
    var paddatepart = function(part)
    {
        return part >= 10 ? part.toString() : '0' + part.toString();
    };
    var id = prompt("Enter ID");
    BX24.callMethod(
        "crm.deal.recurring.update",
        {
            id: id,
            fields:
            {
                "CATEGORY_ID": "2",
                "START_DATE": date2str(nextYear),
                "PARAMS": {
                    "MODE": "single",
                    "SINGLE_BEFORE_START_DATE_TYPE": "day",
                    "SINGLE_BEFORE_START_DATE_VALUE": 5,
                    "OFFSET_BEGINDATE_TYPE": "day",
                    "OFFSET_BEGINDATE_VALUE": 1,
                    "OFFSET_CLOSEDATE_TYPE": "month",
                    "OFFSET_CLOSEDATE_VALUE": 2,
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

{% endlist %}

{% include [Footnote about examples](../../../../_includes/examples.md) %}