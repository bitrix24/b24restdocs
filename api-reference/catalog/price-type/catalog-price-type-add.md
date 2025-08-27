# Create Price Type catalog.priceType.add

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method adds a new price type.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md)| Field values for creating a new price type ([detailed description](#fields)) ||
|#

### Field Parameter {#fields}

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../data-types.md) | Code of the price type.

To ensure the stable operation of internal services, the price type code must be specified using only English characters.
||
|| **base**
[`string`](../../data-types.md) | Indicates whether the price type is base. Possible values:
- `Y` — yes
- `N` — no

Default is `N`.

Only one base price type can exist at a time. When a new base type is added, the previous one will lose this property and cease to be base.
||
|| **sort**
[`integer`](../../data-types.md) | Sorting.

Default is `100`.
||
|| **xmlId**
[`string`](../../data-types.md) | External code.

Can be used to synchronize the current price type with a similar position in an external system.
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
    -d '{"fields":{"name":"Wholesale price","base":"N","sort":10,"xmlId":"wholesale"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.priceType.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"name":"Wholesale price","base":"N","sort":10,"xmlId":"wholesale"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.priceType.add
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'catalog.priceType.add', 
    		{
    			fields: {
    				name: "Wholesale price",
    				base: "N",
    				sort: 10,
    				xmlId: "wholesale"
    			}
    		}
    	);
    	
    	const result = response.getData().result;
    	console.log(result);
    }
    catch( error )
    {
    	console.error(error.ex);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'catalog.priceType.add',
                [
                    'fields' => [
                        'name'  => "Wholesale price",
                        'base'  => "N",
                        'sort'  => 10,
                        'xmlId' => "wholesale",
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error()->ex);
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding price type: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.priceType.add', 
        {
            fields: {
                name: "Wholesale price",
                base: "N",
                sort: 10,
                xmlId: "wholesale"
            }
        },
        function(result) {
            if (result.error())
                console.error(result.error().ex);
            else
                console.log(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'catalog.priceType.add',
        [
            'fields' => [
                'name' => 'Wholesale price',
                'base' => 'N',
                'sort' => 10,
                'xmlId' => 'wholesale'
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
            "base": "N",
            "createdBy": 1,
            "dateCreate": "2024-10-02T17:49:44+02:00",
            "id": 2,
            "modifiedBy": 1,
            "name": "Wholesale price",
            "sort": 10,
            "timestampX": "2024-10-02T17:49:44+02:00",
            "xmlId": "wholesale"
        }
    },
    "time": {
        "start": 1716552521.40908,
        "finish": 1716552521.69852,
        "duration": 0.289434909820557,
        "processing": 0.011207103729248,
        "date_start": "2024-10-02T17:49:44+02:00",
        "date_finish": "2024-10-02T17:49:44+02:00",
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
[`catalog_price_type`](../data-types.md#catalog_price_type) | Object with information about the created price type ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

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
|| `200040300020` | Insufficient permissions to edit
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

- [{#T}](./catalog-price-type-update.md)
- [{#T}](./catalog-price-type-get.md)
- [{#T}](./catalog-price-type-list.md)
- [{#T}](./catalog-price-type-delete.md)
- [{#T}](./catalog-price-type-get-fields.md)