# Create a New Recurring Deal crm.deal.recurring.add

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- examples (in other languages) are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.deal.recurring.add` adds a new setting for a recurring deal.

#|
|| **Parameter** | **Description** ||
|| **fields** | A set of fields – an array of the form `array ("field"=>"value"[, ...])`, containing the values of the deal fields. The required field is `DEAL_ID` [ID of the deal for which the parameter `IS_RECURRING=Y` is set]. 

{% note info %}

To find out the required format of the fields, execute the method [crm.deal.recurring.fields](./crm-deal-recurring-fields.md) and check the format of the received values for these fields.

{% endnote %}
||
|#

## Example

{% include [Example notes](../../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    try
    {
    	const current = new Date();
    	const nextMonth = new Date();
    	const nextYear = new Date();
    	nextMonth.setMonth(current.getMonth() + 1);
    	nextYear.setFullYear(current.getFullYear() + 1);
    
    	const date2str = function(d)
    	{
    		return d.getFullYear() + '-' + paddatepart(1 + d.getMonth()) + '-' + paddatepart(d.getDate()) + 'T' + paddatepart(d.getHours()) + ':' + paddatepart(d.getMinutes()) + ':' + paddatepart(d.getSeconds()) + '+03:00';
    	};
    
    	const paddatepart = function(part)
    	{
    		return part >= 10 ? part.toString() : '0' + part.toString();
    	};
    
    	const response = await $b24.callMethod(
    		"crm.deal.recurring.add",
    		{
    			fields:
    			{
    				"DEAL_ID": "45",
    				"CATEGORY_ID": "1",
    				"IS_LIMIT": "D",
    				"LIMIT_DATE": date2str(nextYear),
    				"START_DATE": date2str(nextMonth),
    				"PARAMS": {
    					"MODE": "multiple",
    					"MULTIPLE_TYPE": "month",
    					"MULTIPLE_INTERVAL": 1,
    					"OFFSET_BEGINDATE_TYPE": "day",
    					"OFFSET_BEGINDATE_VALUE": 1,
    					"OFFSET_CLOSEDATE_TYPE": "month",
    					"OFFSET_CLOSEDATE_VALUE": 2,
    				}
    			}
    		}
    	);
    
    	const result = response.getData().result;
    	console.info("Recurring deal settings added. Record ID - " + result);
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
        $nextMonth = new DateTime();
        $nextYear = new DateTime();
        $nextMonth->setDate($current->format('Y'), $current->format('m') + 1, $current->format('d'));
        $nextYear->setDate($current->format('Y') + 1, $current->format('m'), $current->format('d'));
    
        $date2str = function($d) {
            return $d->format('Y-m-d\TH:i:sP');
        };
    
        $paddatepart = function($part) {
            return $part >= 10 ? $part : '0' . $part;
        };
    
        $response = $b24Service
            ->core
            ->call(
                'crm.deal.recurring.add',
                [
                    'fields' => [
                        'DEAL_ID'               => '45',
                        'CATEGORY_ID'           => '1',
                        'IS_LIMIT'              => 'D',
                        'LIMIT_DATE'            => date2str($nextYear),
                        'START_DATE'            => date2str($nextMonth),
                        'PARAMS'                => [
                            'MODE'                    => 'multiple',
                            'MULTIPLE_TYPE'           => 'month',
                            'MULTIPLE_INTERVAL'       => 1,
                            'OFFSET_BEGINDATE_TYPE'   => 'day',
                            'OFFSET_BEGINDATE_VALUE'  => 1,
                            'OFFSET_CLOSEDATE_TYPE'   => 'month',
                            'OFFSET_CLOSEDATE_VALUE'  => 2,
                        ],
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
            echo 'Error: ' . $result->error();
        } else {
            echo 'Success: Recurring deal settings added. Record ID - ' . $result->data();
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding recurring deal settings: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    var current = new Date();
    var nextMonth = new Date();
    var nextYear = new Date();
    nextMonth.setMonth(current.getMonth() + 1);
    nextYear.setYear(current.getFullYear() + 1);
    var date2str = function(d)
    {
        return d.getFullYear() + '-' + paddatepart(1 + d.getMonth()) + '-' + paddatepart(d.getDate()) + 'T' + paddatepart(d.getHours()) + ':' + paddatepart(d.getMinutes()) + ':' + paddatepart(d.getSeconds()) + '+03:00';
    };
    var paddatepart = function(part)
    {
        return part >= 10 ? part.toString() : '0' + part.toString();
    };
    BX24.callMethod(
        "crm.deal.recurring.add",
        {
            fields:
            {
                "DEAL_ID": "45",
                "CATEGORY_ID": "1",
                "IS_LIMIT": "D",
                "LIMIT_DATE": date2str(nextYear),
                "START_DATE": date2str(nextMonth),
                "PARAMS": {
                    "MODE": "multiple",
                    "MULTIPLE_TYPE": "month",
                    "MULTIPLE_INTERVAL": 1,
                    "OFFSET_BEGINDATE_TYPE": "day",
                    "OFFSET_BEGINDATE_VALUE": 1,
                    "OFFSET_CLOSEDATE_TYPE": "month",
                    "OFFSET_CLOSEDATE_VALUE": 2,
                }
            }
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.info("Recurring deal settings added. Record ID - " + result.data());
        }
    );
    ```

{% endlist %}

{% include [system errors](../../../../_includes/system-errors.md) %}