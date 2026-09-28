# Add a Catalog Product Item to an Order Cart sale.basketitem.addCatalogProduct

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `sale.basketitem.addCatalogProduct` adds a position (item) with a product or service from the catalog module to the cart of an existing order.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | Field values for creating a cart item in the order [(detailed description)](#fields) ||
|#

### Parameter fields {#fields}

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **orderId***
[`sale_order.id`](../data-types.md#sale_order) | Order identifier. It cannot be changed after the item is created.

Retrieve the identifier using the [sale.order.add](../order/sale-order-add.md) or [sale.order.list](../order/sale-order-list.md) method ||
|| **productId***
[`catalog_product.id`](../../catalog/data-types.md#catalog_product) | Identifier of a product, service, or variation from the catalog. It cannot be changed after the item is created.

Retrieve the identifier using the [catalog.product.list](../../catalog/product/catalog-product-list.md) method ||
|| **quantity***
[`double`](../../data-types.md) | Quantity of the product. Pass a value greater than `0`: with `0`, the method creates an item with no price and no name. You can pass a fractional number, for example `1.5` ||
|| **currency***
[`crm_currency.CURRENCY`](../../crm/data-types.md) | Currency of the price, for example `USD`. Must match the currency of the order, otherwise the method returns the `200140400011` error. It cannot be changed after the item is created ||
|| **price**
[`double`](../../data-types.md) | Unit price including discounts and markups.

If the parameter is not passed, Bitrix24 calculates the price from the catalog data. If the parameter is passed, the item gets `customPrice` = `Y`.

If the product has no price in the catalog, pass `price`, otherwise the method returns the `200140400007` error ||
|| **sort**
[`integer`](../../data-types.md) | Position in the order item list. Default is `100` ||
|| **xmlId**
[`string`](../../data-types.md) | External code of the cart item. If not passed, Bitrix24 generates the code itself, for example `bx_662675fba6516` ||
|#

The method takes the name, unit of measure, base price `basePrice`, discount `discountPrice`, weight, and VAT from the product card in the catalog. If you pass them in `fields`, the values are ignored. To set the item name and price manually, use the [sale.basketitem.add](./sale-basket-item-add.md) method.

The full list of fields, with required and read-only flags, is returned by the [sale.basketitem.getFieldsCatalogProduct](./sale-basket-item-get-catalog-product-fields.md) method.

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

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type AddCatalogProductResult = {
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
        properties: object[]
        quantity: number
        reservations: object[]
        sort: number
        type: number
        vatIncluded: string
        vatRate: number | null
        weight: number
        xmlId: string
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<AddCatalogProductResult>({
        method: 'sale.basketitem.addCatalogProduct',
        params: {
          fields: {
            orderId: 5147,
            quantity: 1,
            productId: 4347,
            currency: 'USD',
          },
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
      async function addCatalogProduct() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'sale.basketitem.addCatalogProduct',
            params: {
              fields: {
                orderId: 5147,
                quantity: 1,
                productId: 4347,
                currency: 'USD',
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
          console.info(result.basketItem.id, result.basketItem.name, result.basketItem.price)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addCatalogProduct)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    fields = {
        "orderId": 5147,
        "quantity": 1,
        "productId": 4347,
        "currency": "USD",
    }

    try:
        bitrix_response = client.sale.basketitem.add_catalog_product(
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

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "sale.basketitem.addCatalogProduct", b24.Params{
    	"fields": b24.Params{
    		"orderId":   5147,
    		"quantity":  1,
    		"productId": 4347,
    		"currency":  "EUR",
    	},
    })
    if err != nil {
    	return fmt.Errorf("sale.basketitem.addCatalogProduct: %w", err)
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
            "basePrice": 1234,
            "canBuy": "Y",
            "catalogXmlId": "FUTURE-QUICKBOOKS-CATALOG",
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
[`sale_basket_item`](../data-types.md#sale_basket_item) | The created cart item. Key fields:
- `id` — item identifier, passed to the [sale.basketitem.updateCatalogProduct](./sale-basket-item-update-catalog-product.md), [sale.basketitem.get](./sale-basket-item-get.md), and [sale.basketitem.delete](./sale-basket-item-delete.md) methods
- `name`, `measureCode`, `measureName`, `catalogXmlId`, `productXmlId` — data from the product card in the catalog
- `price`, `basePrice`, `discountPrice` — unit price, price without discounts, and discount amount
- `customPrice` — `Y` if the price is set manually in `price`, `N` if it is calculated from the catalog
- `type` — item type: `2` for a service, `null` for a simple product, a product with variations, and a variation

For the description of all fields, see the [sale_basket_item](../data-types.md#sale_basket_item) type ||
|| **total**
[`integer`](../../data-types.md) | Number of processed records ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "200140400011",
    "error_description": "Currency must be the currency of the order"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `100` | `Could not find value for parameter {fields}`

The `fields` parameter is not passed
||
|| `0` | `Required fields: productId`

Required fields are not passed. The error text lists all missing fields out of `orderId`, `productId`, `quantity`, and `currency`, for example `Required fields: orderId, currency, quantity`. The method also returns other errors, such as fatal errors, with the same code
||
|| `200140400006` | `Module catalog is not exists`

The Trade Catalog module is not installed
||
|| `200140400007` | `basket item is not saved - bad data`

The item was not created: there is no product with this `productId`, the product is inactive, or it has no price in the catalog and `price` is not passed.

In this case, an empty item with no product and no price may remain in the order. Find it using the [sale.basketitem.list](./sale-basket-item-list.md) method and delete it using the [sale.basketitem.delete](./sale-basket-item-delete.md) method
||
|| `200140400009` | `Order not found`

The order with this `orderId` was not found
||
|| `200140400011` | `Currency must be the currency of the order`

The `currency` does not match the order currency
||
|| `200040300010` | Insufficient rights to add the item
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
- [{#T}](./sale-basket-item-update-catalog-product.md)
- [{#T}](./sale-basket-item-get-fields.md)
- [{#T}](./sale-basket-item-get-catalog-product-fields.md)
