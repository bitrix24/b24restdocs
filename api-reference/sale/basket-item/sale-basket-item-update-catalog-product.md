# Change the basket item position (catalog product) of an existing order sale.basketitem.updateCatalogProduct

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method changes the basket item position (catalog product) of an existing order.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`sale_basket_item.id`](../data-types.md) | Identifier of the basket item. Can be obtained using the methods [sale.basketitem.addCatalogProduct](./sale-basket-item-add-catalog-product.md) and [sale.basketitem.list](./sale-basket-item-list.md) ||
|| **fields***
[`object`](../../data-types.md) | Object with modifiable fields ||
|#

### fields Parameter

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **sort**
[`integer`](../../data-types.md) | Position in the order item list ||
|| **quantity***
[`double`](../../data-types.md) | Quantity of the product ||
|| **xmlId**
[`string`](../../data-types.md) | External code of the basket item ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":6783,"fields":{"quantity":4}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webbhook_here**/sale.basketitem.updateCatalogProduct
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":6783,"fields":{"quantity":4},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.basketitem.updateCatalogProduct
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"sale.basketitem.updateCatalogProduct",
    		{
    			id: 6783,
    			fields: {
    				quantity: 4,
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
                'sale.basketitem.updateCatalogProduct',
                [
                    'id'     => 6783,
                    'fields' => [
                        'quantity' => 4,
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your required data processing logic
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating catalog product: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "sale.basketitem.updateCatalogProduct",
        {
            id: 6783,
            fields: {
                quantity: 4,
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
                    console.log(result.data());
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
        'sale.basketitem.updateCatalogProduct',
        [
            'id' => 6783,
            'fields' => [
                'quantity' => 4,
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
        "basketItem": {
            "basePrice": 1234,
            "canBuy": "Y",
            "catalogXmlId": "FUTURE-QUICKBOOKS-CATALOG",
            "currency": "USD",
            "customPrice": "N",
            "dateInsert": "2024-04-22T16:23:43+02:00",
            "dateUpdate": "2024-04-22T16:32:26+02:00",
            "dimensions": "a:3:{s:5:\"WIDTH\";N;s:6:\"HEIGHT\";N;s:6:\"LENGTH\";N;}",
            "discountPrice": 124,
            "id": 6783,
            "measureCode": "796",
            "measureName": "pcs",
            "name": " Design Development ",
            "orderId": 5147,
            "price": 1110,
            "productid": 4347,
            "productXmlId": "4347",
            "properties": [],
            "quantity": 4,
            "reservations": [],
            "sort": 100,
            "type": 2,
            "vatIncluded": "N",
            "vatRate": null,
            "weight": 0,
            "xmlId": "bx_662672ef370c6"
        }
    },
    "total": 1,
    "time": {
        "start": 1713796344.951712,
        "finish": 1713796346.586924,
        "duration": 1.6352121829986572,
        "processing": 0.6428370475769043,
        "date_start": "2024-04-22T16:32:24+02:00",
        "date_finish": "2024-04-22T16:32:26+02:00",
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
|| **basketItem**
[`sale_basket_item`](../data-types.md) | Object with data of the created basket item ||
|| **total**
[`integer`](../../data-types.md) | Number of processed records ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":0,
    "error_description":"error"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200140400006` | `Module catalog is not exists`

The Trade Catalog module is missing
|| 
|| `200140400001` | `basket item is not exists`

Basket item not found
|| 
|| `200140400008` | `Required fields: fields[ORDER_ID]`

Order ID is not specified
|| 
|| `200140400009` | `Order not found`

Order not found
|| 
|| `200140400011` | `Currency must be the currency of the order`

The item's currency does not match the order's currency
|| 
|| `200040300010` | Insufficient permissions to modify
|| 
|| `100` | Required parameters are not specified
||
|| `0` | Other errors (e.g., fatal errors)
||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sale-basket-item-add.md)
- [{#T}](./sale-basket-item-update.md)
- [{#T}](./sale-basket-item-get.md)
- [{#T}](./sale-basket-item-list.md)
- [{#T}](./sale-basket-item-delete.md)
- [{#T}](./sale-basket-item-get-fields.md)
- [{#T}](./sale-basket-item-add-catalog-product.md)
- [{#T}](./sale-basket-item-get-catalog-product-fields.md)