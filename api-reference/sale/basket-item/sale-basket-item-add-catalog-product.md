# Add a position with a product or service from the catalog module to the cart of an existing order sale.basketitem.addCatalogProduct

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method adds a position (item) with a product or service from the catalog module to the cart of an existing order.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | Field values for creating an item (position) in the order cart ||
|#

### Parameter fields

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **orderId***
[`sale_order.id`](../data-types.md) | Order identifier. Can only be specified when creating a cart position.

Must be obtained earlier using the methods [sale.order.add](../order/sale-order-add.md) or [sale.order.list](../order/sale-order-list.md)
 ||
|| **sort**
[`integer`](../../data-types.md) | Position in the order item list ||
|| **productid***
[`catalog_product.id`](../../catalog/data-types.md#catalog_product) | Identifier of the product/variation
||
|| **price**
[`double`](../../data-types.md) | Price considering markups and discounts (see the `customPrice` field below). If not specified, it will be calculated based on catalog data.

The field will be automatically filled if `customPrice !== ‘Y’`
 ||
|| **basePrice**
[`double`](../../data-types.md) | Original price without considering markups and discounts (see the `customPrice` field below). If not specified, it will be calculated based on catalog data.

The field will be automatically filled if `customPrice !== ‘Y’`
 ||
|| **discountPrice**
[`double`](../../data-types.md) | Amount of the final discount or markup (see the `customPrice` field below). If not specified, it will be calculated based on catalog data.

The field will be automatically filled if `customPrice !== ‘Y’`
 ||
|| **currency***
[`crm_currency.CURRENCY`](../../crm/data-types.md) | Currency of the price. Must match the currency of the order ||
|| **customPrice**
[`string`](../../data-types.md) | Is the price specified manually:
- `Y` — price is set manually
- `N` — price is obtained from the product catalog

Default value is `N`.

If `Y` is specified, the catalog price data will be ignored. The parameters `price`, `basePrice`, and `discountPrice` must be explicitly set so that the condition `basePrice = price + discountPrice` is met
 ||
|| **quantity***
[`double`](../../data-types.md) | Quantity of the product ||
|| **xmlId**
[`string`](../../data-types.md) | External code of the cart position ||
|| **name***
[`string`](../../data-types.md) | Name of the product ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"orderId":5147,"quantity":1,"productId":4347,"currency":"USD"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.basketitem.addCatalogProduct
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"orderId":5147,"quantity":1,"productId":4347,"currency":"USD"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.basketitem.addCatalogProduct
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"sale.basketitem.addCatalogProduct",
    		{
    			fields: {
    				orderId: 5147,
    				quantity: 1,
    				productId: 4347,
    				currency: 'USD',
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
                'sale.basketitem.addCatalogProduct',
                [
                    'fields' => [
                        'orderId'   => 5147,
                        'quantity'  => 1,
                        'productId' => 4347,
                        'currency'  => 'USD',
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
        echo 'Error adding catalog product: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "sale.basketitem.addCatalogProduct",
        {
            fields: {
                orderId: 5147,
                quantity: 1,
                productId: 4347,
                currency: 'USD',
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
        'sale.basketitem.addCatalogProduct',
        [
            'fields' => [
                'orderId' => 5147,
                'quantity' => 1,
                'productId' => 4347,
                'currency' => 'USD',
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
            "catalogXmlId": "FUTURE-1C-CATALOG",
            "currency": "USD",
            "customPrice": "N",
            "dateInsert": "2024-04-22T16:36:43+02:00",
            "dateUpdate": "2024-04-22T16:36:43+02:00",
            "dimensions": "a:3:{s:5:\"WIDTH\";N;s:6:\"HEIGHT\";N;s:6:\"LENGTH\";N;}",
            "discountPrice": 124,
            "id": 6784,
            "measureCode": "796",
            "measureName": "pcs",
            "name": "Service2",
            "orderId": 5147,
            "price": 1110,
            "productId": 4347,
            "productXmlId": "4347",
            "properties": [],
            "quantity": 1,
            "reservations": [],
            "sort": 100,
            "type": 2,
            "vatIncluded": "N",
            "vatRate": null,
            "weight": 0,
            "xmlId": "bx_662675fba6516"
        }
    },
    "total": 1,
    "time": {
        "start": 1713796602.830767,
        "finish": 1713796604.315251,
        "duration": 1.4844841957092285,
        "processing": 0.6749260425567627,
        "date_start": "2024-04-22T16:36:42+02:00",
        "date_finish": "2024-04-22T16:36:44+02:00",
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
[`sale_basket_item`](../data-types.md) | Object with data of the created item (position) in the cart ||
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
|| `200140400006` | `Module catalog does not exist`

The Trade Catalog module (catalog) is missing
|| 
|| `200140400007` | `basket item is not saved - bad data`

The item was not created. The error occurs if an incorrect product identifier is passed or if the product is inactive
|| 
|| `200140400008` | `Required fields: fields[ORDER_ID]`

Order identifier is not specified
|| 
|| `200140400009` | `Order not found`

Order not found
|| 
|| `200140400011` | `Currency must be the currency of the order`

The position currency does not match the order currency
|| 
|| `200040300010` | Insufficient rights to add
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
- [{#T}](./sale-basket-item-update-catalog-product.md)
- [{#T}](./sale-basket-item-get-catalog-product-fields.md)