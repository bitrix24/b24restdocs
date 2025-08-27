# Add Measurement Unit crm.measure.add

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method adds a new measurement unit.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`array`](../../data-types.md) | Set of fields — an array of the form `array("field"=>"value"[, ...])`, containing the values of the measurement unit fields.

To find out the required format of the fields, execute the method [crm.measure.fields](./crm-measure-fields.md) and check the format of the received values for these fields 
||
|#

### Parameter fields

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CODE***
[`integer`](../../data-types.md) | Code ||
|| **MEASURE_TITLE***
[`string`](../../data-types.md) | Name of the measurement unit ||
|| **SYMBOL_RUS**
[`string`](../../data-types.md) | Symbol ||
|| **SYMBOL_INTL**
[`string`](../../data-types.md) | International symbol ||
|| **SYMBOL_LETTER_INTL**
[`string`](../../data-types.md) | International letter code ||
|| **IS_DEFAULT**
[`char`](../../data-types.md) | Default ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"CODE":"212","MEASURE_TITLE":"Watt","SYMBOL_RUS":"W","SYMBOL_INTL":"W","SYMBOL_LETTER_INTL":"WTT","IS_DEFAULT":"N"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.measure.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"CODE":"212","MEASURE_TITLE":"Watt","SYMBOL_RUS":"W","SYMBOL_INTL":"W","SYMBOL_LETTER_INTL":"WTT","IS_DEFAULT":"N"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.measure.add
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.measure.add',
    		{
    			fields: {
    				"CODE": "212",
    				"MEASURE_TITLE": "Watt",
    				"SYMBOL_RUS": "W",
    				"SYMBOL_INTL": "W",
    				"SYMBOL_LETTER_INTL": "WTT",
    				"IS_DEFAULT": "N"
    			}
    		}
    	);
    	
    	const result = response.getData().result;
    	console.info('Created measurement unit with ID ' + result);
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
                'crm.measure.add',
                [
                    'fields' => [
                        'CODE'              => '212',
                        'MEASURE_TITLE'     => 'Watt',
                        'SYMBOL_RUS'        => 'W',
                        'SYMBOL_INTL'       => 'W',
                        'SYMBOL_LETTER_INTL' => 'WTT',
                        'IS_DEFAULT'        => 'N',
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Created measurement unit with ID ' . $result;
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error creating measurement unit: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.measure.add",
        {
            fields: {
                "CODE": "212",
                "MEASURE_TITLE": "Watt",
                "SYMBOL_RUS": "W",
                "SYMBOL_INTL": "W",
                "SYMBOL_LETTER_INTL": "WTT",
                "IS_DEFAULT": "N"
            }
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.info("Created measurement unit with ID " + result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.measure.add',
        [
            'fields' =>
            [
                'CODE' => '212',
                'MEASURE_TITLE' => 'Watt',
                'SYMBOL_RUS' => 'W',
                'SYMBOL_INTL' => 'W',
                'SYMBOL_LETTER_INTL' => 'WTT',
                'IS_DEFAULT' => 'N'
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Continue Learning

- [{#T}](./crm-measure-update.md)
- [{#T}](./crm-measure-get.md)
- [{#T}](./crm-measure-list.md)
- [{#T}](./crm-measure-delete.md)
- [{#T}](./crm-measure-fields.md)