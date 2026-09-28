# Get a Cart Item sale.basketitem.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: store manager

The method `sale.basketitem.get` retrieves information about an order cart item by its identifier.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`sale_basket_item.id`](../data-types.md#sale_basket_item) | Identifier of the cart item.

Can be obtained using the [sale.basketitem.list](./sale-basket-item-list.md) method
||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":6801}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.basketitem.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":6801,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.basketitem.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type GetBasketItemResult = {
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
        vatIncluded: string
        vatRate: number | null
        weight: number
        xmlId: string
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<GetBasketItemResult>({
        method: 'sale.basketitem.get',
        params: {
          id: 6801,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.basketItem.id, result.basketItem.name, result.basketItem.price)
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
      async function getBasketItem() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'sale.basketitem.get',
            params: {
              id: 6801,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.basketItem.id, result.basketItem.name, result.basketItem.price)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getBasketItem)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.sale.basketitem.get(
            bitrix_id=6801,
        ).response
        result = bitrix_response.result
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'sale.basketitem.get',
                [
                    'id' => 6801,
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            echo 'Data: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting basket item: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "sale.basketitem.get",
        {
            id: 6801
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
        'sale.basketitem.get',
        [
            'id' => 6801
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "sale.basketitem.get", b24.Params{
    	"id": 6801,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("sale.basketitem.get: %w", err)
    }

    // The method wraps the response in an object with the "basketItem" key.
    raw, ok := b24.Unwrap(res.Result, "basketItem")
    if !ok {
    	return fmt.Errorf("no basketItem key in the response")
    }

    var item struct {
    	BasePrice    int    `json:"basePrice"`
    	CanBuy       string `json:"canBuy"`
    	CatalogXmlID string `json:"catalogXmlId"`
    	Currency     string `json:"currency"`
    	CustomPrice  string `json:"customPrice"`
    	DateInsert   string `json:"dateInsert"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.BasePrice, item.CanBuy)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "basketItem": {
            "barcodeMulti": "N",
            "basePrice": 1000,
            "canBuy": "Y",
            "catalogXmlId": "FUTURE-QUICKBOOKS-CATALOG",
            "currency": "USD",
            "customPrice": "N",
            "dateInsert": "2024-04-23T15:59:37+02:00",
            "dateUpdate": "2024-04-23T15:59:37+02:00",
            "dimensions": "a:3:{s:5:\"WIDTH\";N;s:6:\"HEIGHT\";N;s:6:\"LENGTH\";N;}",
            "discountPrice": 100,
            "id": 6801,
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
            "type": null,
            "vatIncluded": "N",
            "vatRate": null,
            "weight": 0,
            "xmlId": "bx_6627bec8c4fdc"
        }
    },
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
[`sale_basket_item`](../data-types.md#sale_basket_item) | Object containing the cart item data. Key fields:
- `id`, `orderId`, `productId` — identifiers of the cart item, the order, and the product
- `name`, `quantity`, `currency` — product name, quantity, and currency of the cart item
- `price`, `basePrice`, `discountPrice` — unit price, price before discounts, and discount amount
- `customPrice` — `Y` if the price was set manually, `N` if it was calculated from the catalog
- `vatRate`, `vatIncluded` — tax rate and whether the tax is included in the price
- `dateInsert`, `dateUpdate` — dates the cart item was added and last modified
- `properties` — array of [cart item properties](../data-types.md#sale_basket_item_property). If there are no properties, an empty array is returned
- `reservations` — array of [cart item reservations in warehouses](../data-types.md#sale_basket_item_reservation). If there are no reservations, an empty array is returned

All fields are described in the [sale_basket_item](../data-types.md#sale_basket_item) type ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "200140400001",
    "error_description": "basket item is not exists"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200140400001` | `basket item is not exists`

There is no cart item with this `id`
||
|| `200040300010` | Insufficient permissions to read
||
|| `100` | `Bitrix\Sale\BasketItem constructor must be is public`

The `id` parameter is not specified
||
|| `0` | Other errors, such as fatal errors
||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./sale-basket-item-add.md)
- [{#T}](./sale-basket-item-update.md)
- [{#T}](./sale-basket-item-list.md)
- [{#T}](./sale-basket-item-delete.md)
- [{#T}](./sale-basket-item-add-catalog-product.md)
- [{#T}](./sale-basket-item-update-catalog-product.md)
- [{#T}](./sale-basket-item-get-fields.md)
- [{#T}](./sale-basket-item-get-catalog-product-fields.md)
