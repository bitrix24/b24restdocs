# Change the position of the basket in an existing order sale.basketitem.update

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method modifies the position of the basket in an existing order.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`sale_basket_item.id`](../data-types.md) | Identifier of the order item ||
|| **fields***
[`object`](../../data-types.md) | Values of the fields to be modified ||
|#

### Parameter fields

#|
|| **Name**
`type` | **Description** ||
|| **sort**
[`integer`](../../data-types.md) | Position in the list of order items ||
|| **price**
[`double`](../../data-types.md) | Price including markups and discounts ||
|| **basePrice**
[`double`](../../data-types.md) | Original price excluding markups and discounts ||
|| **discountPrice**
[`double`](../../data-types.md) | Amount of the final discount or markup ||
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
[`double`](../../data-types.md) | Tax rate in percentage. To specify the "No VAT" rate, an empty string should be passed ||
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

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type BasketItemUpdateResult = {
      basketItem: {
        basePrice: number
        canBuy: string
        catalogXmlId: string
        currency: string
        customPrice: string
        dateInsert: ISODate | null
        dateUpdate: ISODate | null
        dimensions: string
        discountPrice: number
        id: number
        measureCode: string
        measureName: string
        name: string
        orderId: number
        price: number
        productId: number
        productXmlId: string
        properties: unknown[]
        quantity: number
        reservations: unknown[]
        sort: number
        type: string | null
        vatIncluded: string
        vatRate: number
        weight: number
        xmlId: string
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<BasketItemUpdateResult>({
        method: 'sale.basketitem.update',
        params: {
          id: 6791,
          fields: {
            quantity: 7,
            price: 10,
            discountPrice: 990,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.basketItem.id, result.basketItem.name, result.basketItem.quantity, result.basketItem.price)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function updateBasketItem() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'sale.basketitem.update',
            params: {
              id: 6791,
              fields: {
                quantity: 7,
                price: 10,
                discountPrice: 990,
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.basketItem.id, result.basketItem.name, result.basketItem.quantity, result.basketItem.price)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateBasketItem)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'sale.basketitem.update',
                [
                    'id' => 6791,
                    'fields' => [
                        'quantity'      => 7,
                        'price'         => 10,
                        'discountPrice' => 990,
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating basket item: ' . $e->getMessage();
    }
    ```

- BX24.js

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

- PHP CRest

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

HTTP status: **200**

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
[`sale_basket_item`](../data-types.md) | Object containing data of the updated basket item ||
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
|| `100` | Required parameters not specified
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
