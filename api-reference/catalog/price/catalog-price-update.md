# Update Product Price catalog.price.update

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "Change product sale price" access permission

The method `catalog.price.update` updates the price of a product.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`catalog_price.id`](../data-types.md#catalog_price) | Identifier of the product price ||
|| **fields***
[`object`](../../data-types.md) | Field values for updating the product price ([detailed description](#fields)) ||
|#

### Parameter fields {#fields}

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **price***
[`double`](../../data-types.md) | Price value ||
|| **currency***
[`string`](../../data-types.md) | Currency identifier, can be obtained using the method [crm.currency.list](../../crm/currency/crm-currency-list.md) ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1,"fields":{"currency":"USD","price":5000}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.price.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1,"fields":{"currency":"USD","price":5000},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.price.update
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'catalog.price.update',
    		{
    			id: 1,
    			fields: {
    				currency: "USD",
    				price: 5000
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
                'catalog.price.update',
                [
                    'id' => 1,
                    'fields' => [
                        'currency'       => "USD",
                        'price'          => 5000
                    ]
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
        echo 'Error updating price: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.price.update',
        {
            id: 1,
            fields: {
                currency: "USD",
                price: 5000
            }
        },
        function(result)
        {
            if(result.error())
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
        'catalog.price.update',
        [
            'id' => 1,
            'fields' => [
                'currency' => 'USD',
                'price' => 5000
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
        "price": {
            "catalogGroupId": 1,
            "currency": "USD",
            "extraId": null,
            "id": 1,
            "price": 120.75,
            "productId": 1,
            "quantityFrom": null,
            "quantityTo": null,
            "timestampX": "2024-05-27T12:29:35+02:00"
        }
    },
    "time": {
        "start": 1712327086.69665,
        "finish": 1712327086.95303,
        "duration": 0.256376028060913,
        "processing": 0.0112268924713135,
        "date_start": "2024-05-27T12:29:35+02:00",
        "date_finish": "2024-05-27T12:29:35+02:00",
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
[`catalog_price`](../data-types.md#catalog_price) | Object containing information about the product price ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": 0,
    "error_description":"Required fields: price, currency"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `200040300020` | Access Denied | Insufficient rights to edit the price ||
|| `200040300030` | Access Denied | Insufficient rights to edit the product ||
|| `100` | Could not find value for parameter {fields} | The `fields` parameter is not specified or is empty ||
|| `100` | Could not find value for parameter {id} | The `id` parameter is not specified ||
|| `-` | Price does not exist | No price exists for the product with this identifier ||
|| `0` | Required fields:  | Required fields not provided ||
|| `0` | | Other errors ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-price-add.md)
- [{#T}](./catalog-price-get.md)
- [{#T}](./catalog-price-list.md)
- [{#T}](./catalog-price-delete.md)
- [{#T}](./catalog-price-get-fields.md)
- [{#T}](./catalog-price-modify.md)