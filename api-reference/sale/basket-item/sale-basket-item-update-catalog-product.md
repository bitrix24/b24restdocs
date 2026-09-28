# Update a Catalog Product Item sale.basketitem.updateCatalogProduct

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `sale.basketitem.updateCatalogProduct` changes a cart item with a catalog product in an existing order.

The method changes only the fields `quantity`, `price`, `sort`, and `xmlId`. It takes the name, currency, and product from the catalog and skips the values of these fields in `fields` without an error. To change the item name manually, use the method [sale.basketitem.update](./sale-basket-item-update.md). The method [sale.basketitem.getFieldsCatalogProduct](./sale-basket-item-get-catalog-product-fields.md) returns the list of modifiable fields: their `isReadOnly` and `isImmutable` are `false`.

After the call, the item's `customPrice` field becomes `Y`: the price is fixed and is no longer recalculated from the catalog, even if `price` was not passed.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`sale_basket_item.id`](../data-types.md#sale_basket_item) | Identifier of the cart item. You can retrieve it using the methods [sale.basketitem.addCatalogProduct](./sale-basket-item-add-catalog-product.md) and [sale.basketitem.list](./sale-basket-item-list.md) ||
|| **fields***
[`object`](../../data-types.md) | Object with modifiable fields [(detailed description)](#fields) ||
|#

### fields Parameter {#fields}

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **quantity***
[`double`](../../data-types.md) | Quantity of the product, for example `4` or `1.5`. Always pass it, even if you change only other fields: without it, the method returns the error `Required fields: quantity`.

The method skips `0`, a negative number, or a string without an error, and the quantity stays the same. To remove the item from the order, use the method [sale.basketitem.delete](./sale-basket-item-delete.md) ||
|| **price**
[`double`](../../data-types.md) | Unit price of the product. If not passed, the current item price is retained ||
|| **sort**
[`integer`](../../data-types.md) | Position in the order item list ||
|| **xmlId**
[`string`](../../data-types.md) | External code of the cart item ||
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
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.basketitem.updateCatalogProduct
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":6783,"fields":{"quantity":4},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.basketitem.updateCatalogProduct
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
        barcodeMulti: string,
        basePrice: number,
        canBuy: string,
        catalogXmlId: string,
        currency: string,
        customPrice: string,
        dateInsert: ISODate | null,
        dateUpdate: ISODate | null,
        dimensions: string,
        discountPrice: number,
        id: number,
        measureCode: string,
        measureName: string,
        name: string,
        orderId: number,
        price: number,
        productId: number,
        productXmlId: string,
        properties: Array<{ basketId: number, code: string, id: number, name: string, sort: number, value: string, xmlId: string }>,
        quantity: number,
        reservations: Array<{ basketId: number, dateReserve: ISODate, dateReserveEnd: ISODate, id: number, quantity: number, reservedBy: number | null, storeId: number }>,
        sort: number,
        type: number | null,
        vatIncluded: string,
        vatRate: number | null,
        weight: number,
        xmlId: string,
      },
    }

    try {
      const response = await $b24.actions.v2.call.make<BasketItemUpdateResult>({
        method: 'sale.basketitem.updateCatalogProduct',
        params: {
          id: 6783,
          fields: {
            quantity: 4,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.basketItem.id, result.basketItem.name, result.basketItem.quantity)
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
      async function updateCatalogProduct() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'sale.basketitem.updateCatalogProduct',
            params: {
              id: 6783,
              fields: {
                quantity: 4,
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
          console.info(result.basketItem.id, result.basketItem.name, result.basketItem.quantity)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateCatalogProduct)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    fields = {
        "quantity": 4,
    }

    try:
        bitrix_response = client.sale.basketitem.update_catalog_product(
            bitrix_id=6783,
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

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "sale.basketitem.updateCatalogProduct", b24.Params{
    	"id": 6783,
    	"fields": b24.Params{
    		"quantity": 4,
    	},
    })
    if err != nil {
    	return fmt.Errorf("sale.basketitem.updateCatalogProduct: %w", err)
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
            "basePrice": 100,
            "canBuy": "Y",
            "catalogXmlId": "FUTURE-QUICKBOOKS-CATALOG",
            "currency": "USD",
            "customPrice": "Y",
            "dateInsert": "2026-09-28T06:46:42+02:00",
            "dateUpdate": "2026-09-28T06:46:49+02:00",
            "dimensions": "a:3:{s:5:\"WIDTH\";N;s:6:\"HEIGHT\";N;s:6:\"LENGTH\";N;}",
            "discountPrice": 0,
            "id": 6783,
            "measureCode": "796",
            "measureName": "pcs",
            "name": "Parent product",
            "orderId": 5147,
            "price": 100,
            "productId": 6967,
            "productXmlId": "6967",
            "properties": [],
            "quantity": 4,
            "reservations": [
                {
                    "basketId": 6783,
                    "dateReserve": "2026-09-28T06:46:42+02:00",
                    "dateReserveEnd": "2026-10-01T20:00:00+02:00",
                    "id": 329,
                    "quantity": 1,
                    "reservedBy": null,
                    "storeId": 1
                }
            ],
            "sort": 100,
            "type": null,
            "vatIncluded": "N",
            "vatRate": 0,
            "weight": 0,
            "xmlId": "bx_6ab9ff420a704"
        }
    },
    "total": 1,
    "time": {
        "start": 1790574409,
        "finish": 1790574410.105003,
        "duration": 1.1050031185150146,
        "processing": 1,
        "date_start": "2026-09-28T07:46:49+02:00",
        "date_finish": "2026-09-28T07:46:50+02:00",
        "operating_reset_at": 1790575009,
        "operating": 0.23203706741333008
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
[`sale_basket_item`](../data-types.md#sale_basket_item) | Object with data of the modified cart item. Main fields:
- `id` — identifier of the item
- `orderId` — identifier of the order
- `productId` — identifier of the product in the catalog
- `name` — product name from the catalog
- `quantity` — quantity
- `price` — unit price including discounts and markups, `basePrice` — price excluding them
- `customPrice` — `Y` if the price is fixed manually
- `currency` — price currency
- `properties` — item properties, an array of [sale_basket_item_property](../data-types.md#sale_basket_item_property) objects
- `reservations` — item reservations in warehouses, an array of [sale_basket_item_reservation](../data-types.md#sale_basket_item_reservation) objects

For the full list of fields with types, see the description of the type [sale_basket_item](../data-types.md#sale_basket_item) ||
|| **total**
[`integer`](../../data-types.md) | Number of processed records ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

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
|| `0` | `Required fields: quantity`

The required field `quantity` is not passed in `fields`
||
|| `200140400001` | `basket item is not exists`

There is no cart item with this `id`
||
|| `100` | `Could not find value for parameter {fields}`

The `fields` parameter is not passed
||
|| `100` | `Bitrix\Sale\BasketItem constructor must be is public`

The `id` parameter is not passed
||
|| `200140400006` | `Module catalog is not exists`

The Trade Catalog module is not installed
||
|| `200140400009` | `Order not found`

The item's order is not found
||
|| `200040300010` | Insufficient permissions to modify
||
|| `0` | Other errors (e.g., fatal errors)
||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./sale-basket-item-add.md)
- [{#T}](./sale-basket-item-update.md)
- [{#T}](./sale-basket-item-get.md)
- [{#T}](./sale-basket-item-list.md)
- [{#T}](./sale-basket-item-delete.md)
- [{#T}](./sale-basket-item-add-catalog-product.md)
- [{#T}](./sale-basket-item-get-fields.md)
- [{#T}](./sale-basket-item-get-catalog-product-fields.md)
