# Get Product Price by ID catalog.price.get

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: user with "View product catalog" access permission

The method `catalog.price.get` returns information about the product price by its ID.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`catalog_price.id`](../data-types.md#catalog_price) | Identifier of the product price, can be obtained using the [catalog.price.list](./catalog-price-list.md) method ||
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
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.price.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.price.get
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'catalog.price.get',
    		{
    			id: 1
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
                'catalog.price.get',
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
        echo 'Error getting price: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.price.get',
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
        'catalog.price.get',
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

HTTP status: **200**

```json
{
    "result": {
        "price": {
            "catalogGroupId": 1,
            "currency": "USD",
            "extraId": null,
            "id": 1,
            "price": 100,
            "productId": 1,
            "quantityFrom": null,
            "quantityTo": null,
            "timestampX": "2024-05-27T10:00:00+02:00"
        }
    },
    "time": {
        "start": 1716558215.93495,
        "finish": 1716558216.20731,
        "duration": 0.272363901138306,
        "processing": 0.028817892074585,
        "date_start": "2024-05-27T10:00:00+02:00",
        "date_finish": "2024-05-27T10:00:00+02:00",
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
|| **price**
[`catalog_price`](../data-types.md#catalog_price) | Object containing information about the product price with the specified ID ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": 200040300010,
    "error_description": "Access Denied"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `200040300010` | Access Denied | Insufficient permissions to read ||
|| `-` | Price does not exist | No price exists for the product with this ID ||
|| `100` | Could not find value for parameter {id} | Parameter `id` is not specified || 
|| `0` | | Other errors || 
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-price-add.md)
- [{#T}](./catalog-price-update.md)
- [{#T}](./catalog-price-list.md)
- [{#T}](./catalog-price-delete.md)
- [{#T}](./catalog-price-get-fields.md)
- [{#T}](./catalog-price-modify.md)