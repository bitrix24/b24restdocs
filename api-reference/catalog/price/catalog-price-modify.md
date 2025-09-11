# Update Product Price Collection catalog.price.modify

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "Modify product sale price" access permission

The method `catalog.price.modify` updates the product price collection. It allows adding, updating, and deleting different product prices in a single request.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md)| Object containing data to modify prices ([detailed description](#fields)) ||
|#

### Parameter fields {#fields}

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **product***
[`object`](../../data-types.md)| Product data and its prices ([detailed description](#product)) ||
|#

### Parameter product {#product}

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`catalog_product.id`](../data-types.md#catalog_product) | Product identifier, can be obtained using the methods [catalog.product.list](../product/catalog-product-list.md), [catalog.product.offer.list](../product/offer/catalog-product-offer-list.md), [catalog.product.service.list](../product/service/catalog-product-service-list.md), [catalog.product.sku.list](../product/sku/catalog-product-sku-list.md) ||
|| **prices***
[`array`](../../data-types.md) | Array of product prices ([detailed description](#prices)) ||
|#

### Parameter prices {#prices}

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`catalog_price.id`](../data-types.md#catalog_price) | Price identifier. If specified, the price will be updated. If not specified, a new price will be created. The price whose identifier was not provided will be deleted. Price identifiers can be obtained using the method [catalog.price.list](./catalog-price-list.md) ||
|| **catalogGroupId***
[`catalog_price_type.id`](../data-types.md#catalog_price_type) | Identifier of the price type, can be obtained using the method [catalog.priceType.list](../price-type/catalog-price-type-list.md) ||
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
    -d '{"fields":{"product":{"id":8,"prices":[{"catalogGroupId":1,"currency":"USD","price":2001},{"catalogGroupId":3,"currency":"USD","price":2001},{"catalogGroupId":5,"currency":"USD","price":2001,"id":122}]}}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.price.modify
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"product":{"id":8,"prices":[{"catalogGroupId":1,"currency":"USD","price":2001},{"catalogGroupId":3,"currency":"USD","price":2001},{"catalogGroupId":5,"currency":"USD","price":2001,"id":122}]},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.price.modify
    ```

- JS

    ```javascript
    try {
        const response = await $b24.callMethod(
            'catalog.price.modify',
            {
                fields: {
                    product: {
                        id: 8,
                        prices: [
                            {
                                catalogGroupId: 1,
                                currency: 'USD',
                                price: 2001
                            },
                            {
                                catalogGroupId: 3,
                                currency: 'USD',
                                price: 2001
                            },
                            {
                                catalogGroupId: 5,
                                currency: 'USD',
                                price: 2001,
                                id: 122
                            },
                        ]
                    },
                }
            }
        );

        const result = response.getData().result;
        console.log('Modified prices:', result);
        processResult(result);
    } catch (error) {
        console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'catalog.price.modify',
                [
                    'fields' => [
                        'product' => [
                            'id' => 8,
                            'prices' => [
                                [
                                    'catalogGroupId' => 1,
                                    'currency' => 'USD',
                                    'price' => 2001
                                ],
                                [
                                    'catalogGroupId' => 3,
                                    'currency' => 'USD',
                                    'price' => 2001
                                ],
                                [
                                    'catalogGroupId' => 5,
                                    'currency' => 'USD',
                                    'price' => 2001,
                                    'id' => 122
                                ]
                            ]
                        ]
                    ]
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
        echo 'Error modifying prices: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.price.modify',
        {
            fields: {
                product: {
                    id: 8,
                    prices: [
                        {
                            catalogGroupId: 1,
                            currency: 'USD',
                            price: 2001
                        },
                        {
                            catalogGroupId: 3,
                            currency: 'USD',
                            price: 2001               
                        },
                        {
                            catalogGroupId: 5,
                            currency: 'USD',
                            price: 2001,                
                            id: 122
                        },
                    ]
                },
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
        'catalog.price.modify',
        [
            'fields' => [
                'product' => [
                    'id' => 8,
                    'prices' => [
                        [
                            'catalogGroupId' => 1,
                            'currency' => 'USD',
                            'price' => 2001
                        ],
                        [
                            'catalogGroupId' => 3,
                            'currency' => 'USD',
                            'price' => 2001
                        ],
                        [
                            'catalogGroupId' => 5,
                            'currency' => 'USD',
                            'price' => 2001,
                            'id' => 122
                        },
                    ]
                },
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
        "prices": [
            {
                "catalogGroupId": 1,
                "currency": "USD",
                "extraId": null,
                "id": 123,
                "price": 2001,
                "priceScale":2001,
                "productId": 8,
                "quantityFrom": null,
                "quantityTo": null,
                "timestampX": "2024-05-27T16:29:35+02:00"
            },
            {
                "catalogGroupId": 3,
                "currency": "USD",
                "extraId": null,
                "id": 124,
                "price": 2001,
                "priceScale":2001,
                "productId": 8,
                "quantityFrom": null,
                "quantityTo": null,
                "timestampX": "2024-05-27T16:29:35+02:00"
            },
            {
                "catalogGroupId": 5,
                "currency": "USD",
                "extraId": null,
                "id": 122,
                "price": 2001,
                "priceScale":2001,
                "productId": 8,
                "quantityFrom": null,
                "quantityTo": null,
                "timestampX": "2024-05-27T16:29:35+02:00"
            }
        ]
    },
    "time": {
        "start": 1712327086.69665,
        "finish": 1712327086.95303,
        "duration": 0.256376028060913,
        "processing": 0.0112268924713135,
        "date_start": "2024-05-27T16:29:35+02:00",
        "date_finish": "2024-05-27T16:29:35+02:00",
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
|| **prices**
[`catalog_price[]`](../data-types.md#catalog_price) | Array of objects with product price information ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": 0,
    "error_description": "Required fields: price, currency"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `200040300030` | Access Denied | Insufficient permissions ||
|| `100` | Could not find value for parameter {fields} | Parameter `fields` is not specified or is empty ||
|| `0` | Required fields:  | Required fields are not provided ||
|| `0` | Validate price error. Catalog price group is wrong | Incorrect price type ||
|| `0` | | Other errors || 
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-price-add.md)
- [{#T}](./catalog-price-update.md)
- [{#T}](./catalog-price-get.md)
- [{#T}](./catalog-price-list.md)
- [{#T}](./catalog-price-delete.md)
- [{#T}](./catalog-price-get-fields.md)