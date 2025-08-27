# Update Price Type catalog.priceType.update

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method modifies the values of the price type fields.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`catalog_price_type.id`](../data-types.md#catalog_price_type) | Identifier of the price type ||
|| **fields***
[`object`](../../data-types.md) | Field values to update the price type ([detailed description](#fields)) ||
|#

### Parameter fields {#fields}

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../data-types.md) | Code of the price type ||
|| **base**
[`string`](../../data-types.md) | Indicates if the price type is base. Possible values:
- `Y` — yes
- `N` — no
||
|| **sort**
[`integer`](../../data-types.md) | Sorting ||
|| **xmlId**
[`string`](../../data-types.md) | External code.

Can be used to synchronize the current price type with a similar position in an external system
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
    -d '{"id":2,"fields":{"name":"Base wholesale price","base":"Y","sort":1,"xmlId":"basewholesale"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.priceType.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":2,"fields":{"name":"Base wholesale price","base":"Y","sort":1,"xmlId":"basewholesale"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.priceType.update
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'catalog.priceType.update', 
    		{
    			id: 2,
    			fields: {
    				name: "Base wholesale price",
    				base: "Y",
    				sort: 1,
    				xmlId: "basewholesale"
    			}
    		}
    	);
    	
    	const result = response.getData().result;
    	console.log(result);
    }
    catch(error)
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
                'catalog.priceType.update',
                [
                    'id' => 2,
                    'fields' => [
                        'name'  => "Base wholesale price",
                        'base'  => "Y",
                        'sort'  => 1,
                        'xmlId' => "basewholesale",
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
        echo 'Error updating price type: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.priceType.update', 
        {
            id: 2,
            fields: {
                name: "Base wholesale price",
                base: "Y",
                sort: 1,
                xmlId: "basewholesale"
            }
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
        'catalog.priceType.update',
        [
            'id' => 2,
            'fields' => [
                'name' => 'Base wholesale price',
                'base' => 'Y',
                'sort' => 1,
                'xmlId' => 'basewholesale'
            ]
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
    "result": {
        "priceType": {
            "base": "Y",
            "createdBy": 1,
            "dateCreate": "2024-10-02T17:49:44+02:00",
            "id": 2,
            "modifiedBy": 1,
            "name": "Base wholesale price",
            "sort": 1,
            "timestampX": "2024-10-03T12:29:35+02:00",
            "xmlId": "basewholesale"
        }
    },
    "time": {
        "start": 1712327086.69665,
        "finish": 1712327086.95303,
        "duration": 0.256376028060913,
        "processing": 0.0112268924713135,
        "date_start": "2024-10-03T12:29:35+02:00",
        "date_finish": "2024-10-03T12:29:35+02:00",
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
|| **priceType**
[`catalog_price_type`](../data-types.md#catalog_price_type) | Object with information about the updated price type ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": 0,
    "error_description":"Required fields: name"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300020` | Insufficient permissions to edit
||
|| `201000000000` | Price type with such identifier does not exist
||
|| `100` | Parameter `id` not specified
||
|| `100` | Parameter `fields` not specified or empty
||
|| `0` | Required fields of the `fields` structure not provided
|| 
|| `0` | Other errors (e.g., fatal errors)
|| 
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-price-type-add.md)
- [{#T}](./catalog-price-type-get.md)
- [{#T}](./catalog-price-type-list.md)
- [{#T}](./catalog-price-type-delete.md)
- [{#T}](./catalog-price-type-get-fields.md)