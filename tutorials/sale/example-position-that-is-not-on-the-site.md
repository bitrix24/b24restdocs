# Create an item with a product that does not exist on the site (2 pcs at a price of 900 rubles)

{% include [Footnote about examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"orderId":5147,"productId":0,"name":"Test Product","quantity":2,"basePrice":1000,"price":900,"discountPrice":100,"customPrice":"Y","canBuy":"Y","weight":40,"measureCode":"768","measureName":"pcs","sort":400,"xmlId":"BasketPositionId","dimensions":"a:3:{s:5:\"WIDTH\";i:244;s:6:\"HEIGHT\";i:100;s:6:\"LENGTH\";i:31;}","vatRate":10,"vatIncluded":"Y","productXmlId":"ProductKey","currency":"RUB"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.basketitem.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"orderId":5147,"productId":0,"name":"Test Product","quantity":2,"basePrice":1000,"price":900,"discountPrice":100,"customPrice":"Y","canBuy":"Y","weight":40,"measureCode":"768","measureName":"pcs","sort":400,"xmlId":"BasketPositionId","dimensions":"a:3:{s:5:\"WIDTH\";i:244;s:6:\"HEIGHT\";i:100;s:6:\"LENGTH\";i:31;}","vatRate":10,"vatIncluded":"Y","productXmlId":"ProductKey","currency":"RUB"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.basketitem.add
    ```

- JS

    ```js
    BX24.callMethod(
        "sale.basketitem.add",
        {
            fields: { // all fields are filled
                orderId: 5147,
                productId: 0,
                name: 'Test Product',
                quantity: 2,
                basePrice: 1000,
                price: 900,
                discountPrice: 100,
                customPrice: 'Y',
                canBuy: 'Y',
                weight: 40,
                measureCode: '768',
                measureName: 'pcs',
                sort: 400,
                xmlId: 'BasketPositionId',
                dimensions: 'a:3:{s:5:"WIDTH";i:244;s:6:"HEIGHT";i:100;s:6:"LENGTH";i:31;}', // serialized array
                vatRate: 10,
                vatIncluded: 'Y',
                productXmlId: 'ProductKey',
                currency: 'RUB',
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

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sale.basketitem.add',
        [
            'fields' =>
            [
                'orderId' => 5147,
                'productId' => 0,
                'name' => 'Test Product',
                'quantity' => 2,
                'basePrice' => 1000,
                'price' => 900,
                'discountPrice' => 100,
                'customPrice' => 'Y',
                'canBuy' => 'Y',
                'weight' => 40,
                'measureCode' => '768',
                'measureName' => 'pcs',
                'sort' => 400,
                'xmlId' => 'BasketPositionId',
                'dimensions' => 'a:3:{s:5:"WIDTH";i:244;s:6:"HEIGHT";i:100;s:6:"LENGTH";i:31;}',
                'vatRate' => 10,
                'vatIncluded' => 'Y',
                'productXmlId' => 'ProductKey',
                'currency' => 'RUB',
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Result

```json
{
    "result": {
        "basketItem": {
            "basePrice": 1000,
            "canBuy": "Y",
            "currency": "RUB",
            "customPrice": "Y",
            "dateInsert": "2024-04-23T18:51:28+02:00",
            "dateUpdate": "2024-04-23T18:51:28+02:00",
            "dimensions": "a:3:{s:5:\"WIDTH\";i:244;s:6:\"HEIGHT\";i:100;s:6:\"LENGTH\";i:31;}",
            "discountPrice": 100,
            "id": 6791,
            "measureCode": "768",
            "measureName": "pcs",
            "name": "Test Product",
            "orderId": 5147,
            "price": 900,
            "productId": 0,
            "productXmlId": "ProductKey",
            "properties": [],
            "quantity": 2,
            "reservations": [],
            "sort": 400,
            "vatIncluded": "Y",
            "vatRate": 10,
            "weight": 40,
            "xmlId": "BasketPositionId"
        }
    },
    "total": 1,
    "time": {
        "start": 1713891087.689427,
        "finish": 1713891089.165652,
        "duration": 1.4762251377105713,
        "processing": 0.8683149814605713,
        "date_start": "2024-04-23T18:51:27+02:00",
        "date_finish": "2024-04-23T18:51:29+02:00",
        "operating": 0
    }
}
```