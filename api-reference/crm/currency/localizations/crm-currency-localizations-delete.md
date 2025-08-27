# Delete currency localizations crm.currency.localizations.delete

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user with access to modify CRM settings

This method deletes currency localizations for the specified languages.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
||  **Name**
`type`| **Description** ||
|| **id***
[`string`](../../../data-types.md) | Currency identifier.

Corresponds to the ISO 4217 standard.

The identifier can be obtained using the [crm.currency.list](../crm-currency-list.md) method.
 ||
|| **lids***
[`array`](../../../data-types.md) | Array of language identifiers for which localizations need to be deleted ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":5}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.automatedsolution.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":5,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.automatedsolution.delete
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"crm.currency.localizations.delete",
    		{
    			id: 'CLF',
    			lids: [
    				'en',
    				'de'
    			]
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
                'crm.currency.localizations.delete',
                [
                    'id'   => 'CLF',
                    'lids' => [
                        'en',
                        'de'
                    ]
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error deleting currency localizations: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.currency.localizations.delete",
        {
            id: 'CLF',
            lids: [
                'en',
                'de'
            ]
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
        'crm.automatedsolution.delete',
        [
            'id' => 5
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
        "start": 1718117124.923431,
        "finish": 1718117125.511803,
        "duration": 0.588371992111206,
        "processing": 0.043914079666137695,
        "date_start": "2024-06-11T16:45:24+02:00",
        "date_finish": "2024-06-11T16:45:25+02:00",
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
- `false` — if the operation could not be completed, but there is no error, or the situation is not considered erroneous. Possible reasons:
  - currency module is missing
  - an empty array was passed in the `lids` parameter
  - no localization was deleted
 ||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

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
|| Empty string | Access denied. | Insufficient access rights ||
|| Empty string | The parameter id is invalid or not defined | Empty currency identifier ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./crm-currency-localizations-get.md)
- [{#T}](./crm-currency-localizations-set.md)
- [{#T}](./crm-currency-localizations-fields.md)