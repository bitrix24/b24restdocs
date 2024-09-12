# Update the Basket Item Position of an Existing Order sale.basketitem.update

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method updates the basket item position of an existing order.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`sale_basket_item.id`](../data-types.md) | Identifier of the order item ||
|| **fields***
[`object`](../../data-types.md) | Values of the fields to be updated ||
|#

### Fields Parameter

#|
|| **Name**
`type` | **Description** ||
|| **sort**
[`integer`](../../data-types.md) | Position in the list of order items ||
|| **price**
[`double`](../../data-types.md) | Price including surcharges and discounts ||
|| **basePrice**
[`double`](../../data-types.md) | Original price excluding surcharges and discounts ||
|| **discountPrice**
[`double`](../../data-types.md) | Amount of the final discount or surcharge ||
|| **quantity**
[`double`](../../data-types.md) | Quantity of the product ||
|| **xmlId**
[`string`](../../data-types.md) | External code of the basket item ||
|| **name**
[`string`](../../data-types.md) | Name of the product ||
|| **weight**
[`integer`](../../data-types.md) | Weight of the product ||
|| **dimensions**
[`string`](../../data-types.md) | Dimensions of the product (serialized array) ||
|| **measureCode**
[`catalog_measure.code`](../../catalog/data-types.md#catalog_measure) | Code of the product's unit of measure ||
|| **measureName**
[`catalog_measure.symbol`](../../catalog/data-types.md#catalog_measure) | Name of the unit of measure ||
|| **canBuy**
[`string`](../../data-types.md) | Availability flag of the product. Possible values:
- `Y` — yes
- `N` — no ||
|| **vatRate**
[`double`](../../data-types.md) | Tax rate in percentage. To indicate the "No VAT" rate, an empty string should be passed ||
|| **vatIncluded**
[`string`](../../data-types.md) | Flag indicating whether VAT or tax is included in the product price. Possible values:
- `Y` — yes
- `N` — no ||
|| **catalogXmlId**
[`string`](../../data-types.md) | External code of the product catalog ||
|| **productXmlId**
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
    -d '{"id":6791,"fields":{"quantity":7,"price":10,"discountPrice":990}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.basketitem.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":6791,"fields":{"quantity":7,"price":10,"discountPrice":990},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.basketitem.update
    ```

- JS

    ```js
    BX24.callMethod(
        "sale.basketitem.update",
        {
            id: 6791,
            fields: {
                quantity: 7,
                price: 10,
                discountPrice: 990,
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
                    console.log(result);
                }
            },
            function(error)
            {
                console.info(error);
            }
        );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sale.basketitem.update',
        [
            'id' => 6791,
            'fields' =>
            [
                'quantity' => 7,
                'price' => 10,
                'discountPrice' => 990,
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
        "basketItem": {
            "basePrice": 1000,
            "canBuy": "Y",
            "catalogXmlId": "",
            "currency": "USD",
            "customPrice": "Y",
            "dateInsert": "2024-04-23T18:51:28+02:00",
            "dateUpdate": "2024-04-24T14:02:25+02:00",
            "dimensions": "a:3:{s:5:\"WIDTH\";i:244;s:6:\"HEIGHT\";i:100;s:6:\"LENGTH\";i:31;}",
            "discountPrice": 990,
            "id": 6791,
            "measureCode": "768",
            "measureName": "pcs",
            "name": "Sample Product",
            "orderId": 5147,
            "price": 10,
            "productId": 0,
            "productXmlId": "ProductKey",
            "properties": [],
            "quantity": 7,
            "reservations": [],
            "sort": 400,
            "type": null,
            "vatIncluded": "Y",
            "vatRate": 10,
            "weight": 40,
            "xmlId": "BasketPositionId"
        }
    },
    "total": 1,
    "time": {
        "start": 1713960144.842687,
        "finish": 1713960146.089664,
        "duration": 1.2469770908355713,
        "processing": 0.5501749515533447,
        "date_start": "2024-04-24T14:02:24+02:00",
        "date_finish": "2024-04-24T14:02:26+02:00",
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
[`sale_basket_item`](../data-types.md) | Object containing the data of the updated basket item ||
|| **total**
[`integer`](../../data-types.md) | Number of processed records ||
|| **time**
[`time`](../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

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
|| `200140400001` | `basket item does not exist`

Basket item not found
|| 
|| `200140400008` | `Required fields: fields[ORDER_ID]`

Order ID not specified
|| 
|| `200140400009` | `Order not found`

Order not found
|| 
|| `200140400011` | `Currency must match the order's currency`

Item currency does not match the order's currency
|| 
|| `200040300010` | Insufficient permissions to modify
|| 
|| `100` | Required parameters are missing
||
|| `0` | Other errors (e.g., fatal errors)
|| 
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sale-basket-item-add.md)
- [{#T}](./sale-basket-item-get.md)
- [{#T}](./sale-basket-item-list.md)
- [{#T}](./sale-basket-item-delete.md)
- [{#T}](./sale-basket-item-get-fields.md)
- [{#T}](./sale-basket-item-add-catalog-product.md)
- [{#T}](./sale-basket-item-update-catalog-product.md)
- [{#T}](./sale-basket-item-get-catalog-product-fields.md)