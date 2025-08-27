# Get Values of the Price Rounding Rule Fields catalog.roundingRule.get

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method returns information about the price rounding rule by its identifier.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`catalog_rounding_rule.id`](../data-types.md#catalog_rounding_rule) | Identifier of the price rounding rule ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.roundingRule.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.roundingRule.get
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'catalog.roundingRule.get',
    		{
    			id: 1
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
                'catalog.roundingRule.get',
                [
                    'id' => 1
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
        echo 'Error getting rounding rule: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.roundingRule.get',
        {
            id: 1
        },
        function(result)
        {
            if(result.error())
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
        'catalog.roundingRule.get',
        [
            'id' => 1
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
        "roundingRule": {
            "catalogGroupId": 14,
            "createdBy": 1,
            "dateCreate": "2024-09-25T16:40:59+02:00",
            "dateModify": "2024-09-25T16:40:59+02:00",
            "id": 1,
            "modifiedBy": 1,
            "price": 1000,
            "roundPrecision": 100,
            "roundType": 4
        }
    },
    "time": {
        "start": 1716558215.93495,
        "finish": 1716558216.20731,
        "duration": 0.272363901138306,
        "processing": 0.028817892074585,
        "date_start": "2024-09-25T16:40:59+02:00",
        "date_finish": "2024-09-25T16:40:59+02:00",
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
|| **roundingRule**
[`catalog_rounding_rule`](../data-types.md#catalog_rounding_rule) | Object containing information about the price rounding rule with the specified identifier ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": 200040300010,
    "error_description": "Access Denied"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300010` | Insufficient rights to read
||
|| `200900000000` | No price rounding rule exists with this identifier 
||
|| `100` | The `id` parameter is not specified
|| 
|| `0` | Other errors (e.g., fatal errors)
|| 
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./catalog-rounding-rule-add.md)
- [{#T}](./catalog-rounding-rule-update.md)
- [{#T}](./catalog-rounding-rule-list.md)
- [{#T}](./catalog-rounding-rule-delete.md)
- [{#T}](./catalog-rounding-rule-get-fields.md)