# Add Item (Position) to the Cart of an Existing Order sale.basketitem.add

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method adds an item to the cart of an existing order.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | Field values for creating an item (position) in the cart of the order ||
|#

### Parameter fields

{% include [Note on required parameters](../../../_includes/required.md) %}

Field values marked with ** will be taken from the product data on the site if a valid product identifier is passed in the `productid` field. If the product does not exist on the site, the field must be filled in manually. {.b24-info}

#|
|| **Name**
`type` | **Description** ||
|| **orderId***
[`sale_order.id`](../data-types.md) | Order identifier ||
|| **sort**
[`integer`](../../data-types.md) | Position in the list of order items ||
|| **productid***
[`catalog_product.id`](../../catalog/data-types.md#catalog_product) | Product/variation identifier.

For products that are not on the site/account, it may be zero
 ||
|| **price**
[`double`](../../data-types.md) | Price including markups and discounts (see the `customPrice` field below).

The field will be filled automatically if `customPrice !== ‘Y’`
 ||
|| **basePrice**
[`double`](../../data-types.md) | Original price excluding markups and discounts (see the `customPrice` field below).

The field will be filled automatically if `customPrice !== ‘Y’`
 ||
|| **discountPrice**
[`double`](../../data-types.md) | Amount of the final discount or markup (see the `customPrice` field below).

The field will be filled automatically if `customPrice !== ‘Y’`
 ||
|| **currency***
[`crm_currency.CURRENCY`](../../crm/data-types.md) | Currency of the price. Must match the currency of the order ||
|| **customPrice**
[`string`](../../data-types.md) | Is the price specified manually. Possible values:
- `Y` — yes
- `N` — no

If `Y` is specified, catalog data will be ignored. The parameters `price`, `basePrice`, and `discountPrice` must be explicitly set so that the condition `basePrice = price + discountPrice` is met
 ||
|| **quantity***
[`double`](../../data-types.md) | Quantity of the product ||
|| **xmlId**
[`string`](../../data-types.md) | External code of the cart item ||
|| **name***,**
[`string`](../../data-types.md) | Product name ||
|| **weight****
[`integer`](../../data-types.md) | Weight of the product ||
|| **dimensions****
[`string`](../../data-types.md) | Dimensions of the product (serialized array) ||
|| **measureCode****
[`catalog_measure.code`](../../catalog/data-types.md#catalog_measure) | Unit code of the product ||
|| **measureName****
[`catalog_measure.symbol`](../../catalog/data-types.md#catalog_measure) | Name of the unit of measure ||
|| **canBuy****
[`string`](../../data-types.md) | Availability flag of the product. Possible values:
- `Y` — yes
- `N` — no ||
|| **vatRate****
[`double`](../../data-types.md) | Tax rate in percentage. To specify the rate "No VAT", an empty string must be passed ||
|| **vatIncluded****
[`string`](../../data-types.md) | Flag indicating whether VAT or tax is included in the product price. Possible values:
- `Y` — yes
- `N` — no ||
|| **catalogXmlId****
[`string`](../../data-types.md) | External code of the product catalog ||
|| **productXmlId****
[`string`](../../data-types.md) | External code of the product ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"orderId":5147,"quantity":2,"productId":6544,"currency":"USD"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.basketitem.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"orderId":5147,"quantity":2,"productId":6544,"currency":"USD"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.basketitem.add
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"sale.basketitem.add",
    		{
    			fields: { // minimum set of required fields
    				orderId: 5147,
    				quantity: 2,
    				productId: 6544,
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
                'sale.basketitem.add',
                [
                    'fields' => [
                        'orderId'   => 5147,
                        'quantity'  => 2,
                        'productId' => 6544,
                        'currency'  => 'USD',
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding basket item: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "sale.basketitem.add",
        {
            fields: { // minimum set of required fields
                orderId: 5147,
                quantity: 2,
                productId: 6544,
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
        'sale.basketitem.add',
        [
            'fields' =>
            [
                'orderId' => 5147,
                'quantity' => 2,
                'productId' => 6544,
                'currency' => 'USD',
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

{% note tip "Typical use-cases and scenarios" %}

- [{#T}](../../../tutorials/sale/example-position-with-custom-price.md)
- [{#T}](../../../tutorials/sale/example-position-that-is-not-on-the-site.md)

{% endnote %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "basketItem": {
            "basePrice": 1000,
            "canBuy": "Y",
            "catalogXmlId": "FUTURE-QUICKBOOKS-CATALOG",
            "currency": "USD",
            "customPrice": "N",
            "dateInsert": "2024-04-23T15:59:37+02:00",
            "dateUpdate": "2024-04-23T15:59:37+02:00",
            "dimensions": "a:3:{s:5:\"WIDTH\";N;s:6:\"HEIGHT\";N;s:6:\"LENGTH\";N;}",
            "discountPrice": 100,
            "id": 6790,
            "measureCode": "163",
            "measureName": "g",
            "name": "Product",
            "orderId": 5147,
            "price": 900,
            "productId": 1245,
            "productXmlId": "1245",
            "properties": [],
            "quantity": 1,
            "reservations": [],
            "sort": 100,
            "vatIncluded": "N",
            "vatRate": null,
            "weight": 0,
            "xmlId": "bx_6627bec8c4fdc"
        }
    },
    "total": 1,
    "time": {
        "start": 1713880776.108755,
        "finish": 1713880777.704221,
        "duration": 1.595465898513794,
        "processing": 0.973701000213623,
        "date_start": "2024-04-23T15:59:36+02:00",
        "date_finish": "2024-04-23T15:59:37+02:00",
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
[`time`](../../data-types.md) | Information about the execution time of the request ||
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
|| `200140400007` | `basket item is not saved - bad data`

The item was not created. The error occurs if an invalid product identifier is passed or if the product is inactive
|| 
|| `200140400008` | `Required fields: fields[ORDER_ID]`

Order identifier is not specified
|| 
|| `200140400009` | `Order not found`

Order not found
|| 
|| `200140400011` | `Currency must be the currency of the order`

The currency of the item does not match the currency of the order
|| 
|| `200040300010` | Insufficient permissions to add
|| 
|| `100` | Required parameters are not specified
||
|| `0` | Other errors (e.g., fatal errors)
|| 
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sale-basket-item-update.md)
- [{#T}](./sale-basket-item-get.md)
- [{#T}](./sale-basket-item-list.md)
- [{#T}](./sale-basket-item-delete.md)
- [{#T}](./sale-basket-item-get-fields.md)
- [{#T}](./sale-basket-item-add-catalog-product.md)
- [{#T}](./sale-basket-item-update-catalog-product.md)
- [{#T}](./sale-basket-item-get-catalog-product-fields.md)