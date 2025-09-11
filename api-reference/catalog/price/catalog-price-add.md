# Add Product Price catalog.price.add

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "Change product sale price" access permission

The method `catalog.price.add` adds a new price for a product.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md)| Field values for creating a new product price ([detailed description](#fields)) ||
|#

### Parameter fields {#fields}

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **productId***
[`catalog_product.id`](../data-types.md#catalog_product) | Product identifier, can be obtained using the methods [catalog.product.list](../product/catalog-product-list.md), [catalog.product.offer.list](../product/offer/catalog-product-offer-list.md), [catalog.product.service.list](../product/service/catalog-product-service-list.md), [catalog.product.sku.list](../product/sku/catalog-product-sku-list.md) ||
|| **catalogGroupId***
[`catalog_price_type.id`](../data-types.md#catalog_price_type) | Price type identifier, can be obtained using the method [catalog.priceType.list](../price-type/catalog-price-type-list.md) ||
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
    -d '{"fields":{"catalogGroupId":1,"currency":"USD","price":2000,"productId":1}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.price.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"catalogGroupId":1,"currency":"USD","price":2000,"productId":1},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.price.add
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'catalog.price.add',
    		{
    			fields: {
    				catalogGroupId: 1,
    				currency: "USD",
    				price: 2000,
    				productId: 1
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
                'catalog.price.add',
                [
                    'fields' => [
                        'catalogGroupId' => 1,
                        'currency'       => "USD",
                        'price'          => 2000,
                        'productId'      => 1
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
        echo 'Error adding price: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.price.add',
        {
            fields: {
                catalogGroupId: 1,
                currency: "USD",
                price: 2000,
                productId: 1
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
        'catalog.price.add',
        [
            'fields' => [
                'catalogGroupId' => 1,
                'currency' => 'USD',
                'price' => 2000,
                'productId' => 1
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
            "id": 3,
            "price": 100.5,
            "productId": 1,
            "quantityFrom": null,
            "quantityTo": null,
            "timestampX": "2024-05-27T15:49:44+02:00"
        }
    },
    "time": {
        "start": 1716552521.40908,
        "finish": 1716552521.69852,
        "duration": 0.289434909820557,
        "processing": 0.011207103729248,
        "date_start": "2024-05-27T15:49:44+02:00",
        "date_finish": "2024-05-27T15:49:44+02:00",
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
[`catalog_price`](../data-types.md#catalog_price) | Object containing information about the created product price ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
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
|| **Code** | **Description** | **Value** ||
|| `200040300020` | Access Denied | Insufficient permissions ||
|| `100` | Could not find value for parameter {fields} | Parameter `fields` is missing or empty || 
|| `0` | Required fields: | Required fields are not set ||
|| `0` | Validate price error. Catalog price group is wrong | Incorrect price type ||
|| `0` | Validate price error. Catalog product is allowed has only single price without ranges in price group | A price of this type already exists for the product ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./catalog-price-update.md)
- [{#T}](./catalog-price-get.md)
- [{#T}](./catalog-price-list.md)
- [{#T}](./catalog-price-delete.md)
- [{#T}](./catalog-price-get-fields.md)
- [{#T}](./catalog-price-modify.md)