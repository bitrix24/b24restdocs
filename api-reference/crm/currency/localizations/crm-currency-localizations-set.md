# Set Localizations for Currency crm.currency.localizations.set

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user with access to modify CRM settings

This method updates localizations for a currency or adds them if the localization for the specified language does not exist.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
||  **Name** /
`type`| **Description** ||
|| **id***
[`string`](../../../data-types.md) | Currency identifier.

Corresponds to the ISO 4217 standard.

The identifier can be obtained using the [crm.currency.list](../crm-currency-list.md) method
 ||
|| **localizations***
[`object`](../../../data-types.md) | Currency localization parameters.
An object in the format `{"lang_1": "value_1", ... "lang_N": "value_N"}`, where `lang_N` is the language identifier for which to add/change the localization, and `value` is an object of type [crm_currency_localization](../../data-types.md#crm_currency_localization).

Existing localizations that are not passed to the method will not be changed.
  ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":"CLF","localizations":{"en":{"FULL_NAME":"Unidad de Fomento","FORMAT_STRING":"CLF#VALUE#","DEC_POINT":".","THOUSANDS_VARIANT":"C","DECIMALS":4},"de":{"FULL_NAME":"Einheit der Entwicklung","FORMAT_STRING":"#VALUE# CLF","DEC_POINT":".","THOUSANDS_VARIANT":"B","DECIMALS":4}}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.currency.localizations.set
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":"CLF","localizations":{"en":{"FULL_NAME":"Unidad de Fomento","FORMAT_STRING":"CLF#VALUE#","DEC_POINT":".","THOUSANDS_VARIANT":"C","DECIMALS":4},"de":{"FULL_NAME":"Einheit der Entwicklung","FORMAT_STRING":"#VALUE# CLF","DEC_POINT":".","THOUSANDS_VARIANT":"B","DECIMALS":4}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.currency.localizations.set
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"crm.currency.localizations.set",
    		{
    			id: 'CLF',
    			localizations: {
    				en: {
    					FULL_NAME: 'Unidad de Fomento',
    					FORMAT_STRING: 'CLF#VALUE#',
    					DEC_POINT: '.',
    					THOUSANDS_VARIANT: 'C',
    					DECIMALS: 4,
    				},
    				de: {
    					FULL_NAME: 'Einheit der Entwicklung',
    					FORMAT_STRING: '#VALUE# CLF',
    					DEC_POINT: '.',
    					THOUSANDS_VARIANT: 'B',
    					DECIMALS: 4,
    				}
    			}
    		}
    	);
    	
    	const result = response.getData().result;
    	console.log(result);
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
                'crm.currency.localizations.set',
                [
                    'id' => 'CLF',
                    'localizations' => [
                        'en' => [
                            'FULL_NAME'        => 'Unidad de Fomento',
                            'FORMAT_STRING'    => 'CLF#VALUE#',
                            'DEC_POINT'        => '.',
                            'THOUSANDS_VARIANT' => 'C',
                            'DECIMALS'         => 4,
                        ],
                        'de' => [
                            'FULL_NAME'        => 'Einheit der Entwicklung',
                            'FORMAT_STRING'    => '#VALUE# CLF',
                            'DEC_POINT'        => '.',
                            'THOUSANDS_VARIANT' => 'B',
                            'DECIMALS'         => 4,
                        ],
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error setting currency localizations: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.currency.localizations.set",
        {
            id: 'CLF',
            localizations: {
                en: {
                    FULL_NAME: 'Unidad de Fomento',
                    FORMAT_STRING: 'CLF#VALUE#',
                    DEC_POINT: '.',
                    THOUSANDS_VARIANT: 'C',
                    DECIMALS: 4,
                },
                de: {
                    FULL_NAME: 'Einheit der Entwicklung',
                    FORMAT_STRING: '#VALUE# CLF',
                    DEC_POINT: '.',
                    THOUSANDS_VARIANT: 'B',
                    DECIMALS: 4,
                }
            }
        },
    )
    .then(
        function(result)
        {
            if (result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.log(result);
            }
        },
        function(error)
        {
            console.info(error);
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.currency.localizations.set',
        [
            'id' => 'CLF',
            'localizations' => [
                'en' => [
                    'FULL_NAME' => 'Unidad de Fomento',
                    'FORMAT_STRING' => 'CLF#VALUE#',
                    'DEC_POINT' => '.',
                    'THOUSANDS_VARIANT' => 'C',
                    'DECIMALS' => 4,
                ],
                'de' => [
                    'FULL_NAME' => 'Einheit der Entwicklung',
                    'FORMAT_STRING' => '#VALUE# CLF',
                    'DEC_POINT' => '.',
                    'THOUSANDS_VARIANT' => 'B',
                    'DECIMALS' => 4,
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
        "start": 1718122481.837301,
        "finish": 1718122483.141736,
        "duration": 1.3044350147247314,
        "processing": 0.08866286277770996,
        "date_start": "2024-06-11T18:14:41+02:00",
        "date_finish": "2024-06-11T18:14:43+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Returns:
- `true` — on success
- `false` – if the operation could not be completed, but there is no error, or the situation is not considered erroneous. Possible scenarios:
  - currency module is missing
  - an empty object with localizations was passed
  - no localization was added/changed
 ||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "The parameter id is invalid or not defined."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| Empty string | Access denied. | Insufficient access permissions ||
|| Empty string | The parameter id is invalid or not defined. | Empty currency identifier ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./crm-currency-localizations-get.md)
- [{#T}](./crm-currency-localizations-delete.md)
- [{#T}](./crm-currency-localizations-fields.md)