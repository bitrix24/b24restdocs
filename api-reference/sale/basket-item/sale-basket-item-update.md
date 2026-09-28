# Update an Order Cart Item sale.basketitem.update

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `sale.basketitem.update` modifies a cart item in an existing order. After the quantity or price changes, Bitrix24 recalculates the order total.

For a cart item with a product from the catalog, use the method [sale.basketitem.updateCatalogProduct](./sale-basket-item-update-catalog-product.md): it takes the name and currency of the item from the catalog.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`sale_basket_item.id`](../data-types.md#sale_basket_item) | Identifier of the cart item. You can retrieve cart item identifiers using the method [sale.basketitem.list](./sale-basket-item-list.md) ||
|| **fields***
[`object`](../../data-types.md) | Values of the fields to be modified [(detailed description)](#fields) ||
|#

### Parameter fields {#fields}

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **quantity***
[`double`](../../data-types.md) | Quantity of the product. Pass it in every call, even if the quantity does not change, otherwise the method returns the error `Required fields: quantity`.

The method ignores a zero, negative, or non-numeric value without an error ||
|| **sort**
[`integer`](../../data-types.md) | Position in the list of cart items ||
|| **price**
[`double`](../../data-types.md) | Price including markups and discounts. If you pass a price, Bitrix24 sets `customPrice` = `Y` for the item — the price is considered manually set and is no longer taken from the catalog ||
|| **basePrice**
[`double`](../../data-types.md) | Original price excluding markups and discounts. Pass it together with `price` and `discountPrice` so that `basePrice = price + discountPrice` holds ||
|| **discountPrice**
[`double`](../../data-types.md) | Amount of the final discount or markup. For a markup, the value is negative ||
|| **xmlId**
[`string`](../../data-types.md) | External code of the cart item ||
|| **name**
[`string`](../../data-types.md) | Name of the product ||
|| **weight**
[`double`](../../data-types.md) | Weight of the product. A cart item from the catalog receives the value of the product's `weight` field without conversion, so pass the weight in the same units as in the catalog ||
|| **dimensions**
[`string`](../../data-types.md) | Dimensions of the product — a string with a PHP-serialized array with the keys `WIDTH`, `HEIGHT`, and `LENGTH`, for example `a:3:{s:5:"WIDTH";i:244;s:6:"HEIGHT";i:100;s:6:"LENGTH";i:31;}`. Cart items added from the catalog store dimensions in this form.

The method does not validate the format and saves any string as is. If you pass an object instead of a string, the value `Array` is saved ||
|| **measureCode**
[`catalog_measure.code`](../../catalog/data-types.md#catalog_measure) | Code of the product's unit of measure ||
|| **measureName**
[`catalog_measure.symbol`](../../catalog/data-types.md#catalog_measure) | Name of the unit of measure ||
|| **canBuy**
[`string`](../../data-types.md) | Availability flag of the product. Possible values:
- `Y` — yes
- `N` — no ||
|| **vatRate**
[`double`](../../data-types.md) | Tax rate as a fraction of one: `0.1` means 10 %. For the "No VAT" rate, pass an empty string; the response contains `null` ||
|| **vatIncluded**
[`string`](../../data-types.md) | Flag indicating whether VAT or tax is included in the product price. Possible values:
- `Y` — yes
- `N` — no ||
|#

The fields `orderId`, `productId`, `currency`, `customPrice`, `catalogXmlId`, and `productXmlId` cannot be changed via `sale.basketitem.update`. The method ignores them without an error and returns the previous values. The item currency `currency` always matches the order currency; it is set when the item is added.

To move a product to another order or replace a product, delete the item using the method [sale.basketitem.delete](./sale-basket-item-delete.md) and add a new one using the method [sale.basketitem.add](./sale-basket-item-add.md).

The method also skips unknown fields without an error. Check the result against the `basketItem` object in the response.

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
        barcodeMulti: string
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
        vatRate: number | null
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

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    fields = {
        "quantity": 7,
        "price": 10,
        "discountPrice": 990,
    }

    try:
        bitrix_response = client.sale.basketitem.update(
            bitrix_id=6791,
            fields=fields,
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

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "sale.basketitem.update", b24.Params{
    	"id": 6791,
    	"fields": b24.Params{
    		"quantity":      7,
    		"price":         10,
    		"discountPrice": 990,
    	},
    })
    if err != nil {
    	return fmt.Errorf("sale.basketitem.update: %w", err)
    }

    // The method wraps the response in an object with the "basketItem" key.
    raw, ok := b24.Unwrap(res.Result, "basketItem")
    if !ok {
    	return fmt.Errorf("no basketItem key in the response")
    }

    var item struct {
    	BasePrice    float64 `json:"basePrice"`
    	CanBuy       string  `json:"canBuy"`
    	CatalogXmlID string  `json:"catalogXmlId"`
    	Currency     string  `json:"currency"`
    	CustomPrice  string  `json:"customPrice"`
    	DateInsert   string  `json:"dateInsert"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.BasePrice, item.CanBuy)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "basketItem": {
            "barcodeMulti": "N",
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
            "vatRate": 0.1,
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
[`sale_basket_item`](../data-types.md#sale_basket_item) | Updated cart item. Contains all fields of the item, not only the changed ones. Key fields:
- `id`, `orderId`, `productId` — identifiers of the item, order, and product
- `name`, `quantity`, `currency` — product name, quantity, and item currency
- `price`, `basePrice`, `discountPrice` — unit price, price without discounts, and discount amount
- `customPrice` — `Y` if the price is set manually, `N` if it is calculated from the catalog
- `weight`, `dimensions` — weight and dimensions as they are saved
- `properties` — array of [cart item properties](../data-types.md#sale_basket_item_property)
- `reservations` — array of [cart item reservations in warehouses](../data-types.md#sale_basket_item_reservation)

For a description of all fields, see the type [sale_basket_item](../data-types.md#sale_basket_item) ||
|| **total**
[`integer`](../../data-types.md) | Number of processed records ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "0",
    "error_description": "Required fields: quantity"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `0` | `Required fields: quantity`

The required field `quantity` is not passed in `fields`
||
|| `200140400001` | `basket item is not exists`

There is no cart item with this `id`
||
|| `200040300010` | Insufficient permissions to modify
||
|| `100` | `Could not find value for parameter {fields}`

The `fields` parameter is not passed
||
|| `100` | `Bitrix\Sale\BasketItem constructor must be is public`

The `id` parameter is not passed
||
|| `0` | Other errors (e.g., fatal errors)
||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./sale-basket-item-add.md)
- [{#T}](./sale-basket-item-get.md)
- [{#T}](./sale-basket-item-list.md)
- [{#T}](./sale-basket-item-delete.md)
- [{#T}](./sale-basket-item-add-catalog-product.md)
- [{#T}](./sale-basket-item-update-catalog-product.md)
- [{#T}](./sale-basket-item-get-fields.md)
- [{#T}](./sale-basket-item-get-catalog-product-fields.md)
