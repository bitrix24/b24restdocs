# Add Measurement Unit catalog.measure.add

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method adds a new measurement unit.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | Field values for creating a new measurement unit ||
|#

### Parameter fields

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **code***
[`integer`](../../data-types.md) | Unique code for the measurement unit ||
|| **isDefault**
[`string`](../../data-types.md) | Indicates whether the current measurement unit is used as the default measurement unit for new products. Possible values:
- `Y` — yes
- `N` — no

If the field value is not specified, it is automatically set to `N`.

Only one measurement unit from the entire directory can have the value `Y`
||
|| **measureTitle***
[`string`](../../data-types.md) | Name of the measurement unit
||
|| **symbol**
[`string`](../../data-types.md) | Symbol 
||
|| **symbolIntl**
[`string`](../../data-types.md) | International symbol
||
|| **symbolLetterIntl**
[`string`](../../data-types.md) | International letter code symbol
||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"code":715,"measureTitle":"Pair","symbol":"pair","symbolLetterIntl":"NPR","symbolIntl":"pr; 2","isDefault":"N"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.measure.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"code":715,"measureTitle":"Pair","symbol":"pair","symbolLetterIntl":"NPR","symbolIntl":"pr; 2","isDefault":"N"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.measure.add
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'catalog.measure.add', 
    		{
    			fields: {
    				code: 715,
    				measureTitle: "Pair",
    				symbol: 'pair',
    				symbolLetterIntl: 'NPR',
    				symbolIntl: 'pr; 2',
    				isDefault: 'N'
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
                'catalog.measure.add',
                [
                    'fields' => [
                        'code'            => 715,
                        'measureTitle'    => "Pair",
                        'symbol'          => 'pair',
                        'symbolLetterIntl' => 'NPR',
                        'symbolIntl'      => 'pr; 2',
                        'isDefault'       => 'N',
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding measure: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.measure.add', 
        {
            fields: {
                code: 715,
                measureTitle: "Pair",
                symbol: 'pair',
                symbolLetterIntl: 'NPR',
                symbolIntl: 'pr; 2',
                isDefault: 'N'
            }
        },
        function(result) {
            if (result.error())
                console.error(result.error());
            else
                console.log(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'catalog.measure.add',
        [
            'fields' => [
                'code' => 715,
                'measureTitle' => "Pair",
                'symbol' => 'pair',
                'symbolLetterIntl' => 'NPR',
                'symbolIntl' => 'pr; 2',
                'isDefault' => 'N'
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
    "result": {
        "measure": {
            "code": 715,
            "id": 6,
            "isDefault": "N",
            "measureTitle": "Pair",
            "symbol": "pair",
            "symbolIntl": "pr; 2",
            "symbolLetterIntl": "NPR"
        }
    },
    "time": {
        "start": 1716552521.40908,
        "finish": 1716552521.69852,
        "duration": 0.289434909820557,
        "processing": 0.011207103729248,
        "date_start": "2024-05-24T14:08:41+02:00",
        "date_finish": "2024-05-24T14:08:41+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **measure**
[`catalog_measure`](../data-types.md#catalog_measure) | Object containing information about the created measurement unit ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": 200040300020,
    "error_description": "Access Denied"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300020` | No access to edit
||
|| `200600000000` | Measurement unit with the specified `code` parameter already exists
||
|| `200600000010` | Measurement unit with the `isDefault` parameter set to `Y` already exists
||
|| `100` | Required parameter `fields` not provided
||
|| `0` | Required fields not set
||
|| `0` | Other errors (e.g., fatal errors)
|| 
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-measure-update.md)
- [{#T}](./catalog-measure-get.md)
- [{#T}](./catalog-measure-list.md)
- [{#T}](./catalog-measure-delete.md)
- [{#T}](./catalog-measure-get-fields.md)