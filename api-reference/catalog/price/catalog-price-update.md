# Update Product Price catalog.price.update

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "Change product sale price" access permission and permission to modify product prices in the catalog information block

The method `catalog.price.update` updates the price of a product.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`catalog_price.id`](../data-types.md#catalog_price) | Identifier of the product price. You can retrieve it using the [catalog.price.list](./catalog-price-list.md) method ||
|| **fields***
[`object`](../../data-types.md) | Field values for updating the product price ([detailed description](#fields)) ||
|#

### Parameter fields {#fields}

Pass only the fields you want to change. The method takes the remaining values from the current price.

#|
|| **Name**
`type` | **Description** ||
|| **price**
[`double`](../../data-types.md) | Price value. A negative value causes an error ||
|| **currency**
[`string`](../../data-types.md) | Currency code, for example, `USD`. You can retrieve currency codes using the [crm.currency.list](../../crm/currency/crm-currency-list.md) method ||
|| **extraId**
[`catalog_extra.id`](../data-types.md#catalog_extra) | Markup identifier. You can retrieve the identifier using the [catalog.extra.list](../extra/catalog-extra-list.md) method.

The markup is retained in the price but does not recalculate it. A base type price does not retain a markup. A non-existent identifier causes an error ||
|| **quantityFrom**
[`integer`](../../data-types.md) | Lower bound of the product quantity range. The ranges of all product price types must match the ranges of the base price ||
|| **quantityTo**
[`integer`](../../data-types.md) | Upper bound of the product quantity range ||
|#

The `productId` and `catalogGroupId` fields cannot be changed: if you pass them in `fields`, the method completes without an error, but the values remain unchanged. To move a price to another product or price type, delete it using the [catalog.price.delete](./catalog-price-delete.md) method and create a new one using the [catalog.price.add](./catalog-price-add.md) method.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1,"fields":{"currency":"USD","price":5000}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.price.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1,"fields":{"currency":"USD","price":5000},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.price.update
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type PriceUpdateResult = {
      price: {
        catalogGroupId: number
        currency: string
        extraId: number | null
        id: number
        price: number
        productId: number
        quantityFrom: number | null
        quantityTo: number | null
        timestampX: ISODate | null
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<PriceUpdateResult>({
        method: 'catalog.price.update',
        params: {
          id: 1,
          fields: {
            currency: 'USD',
            price: 5000,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.price.id, result.price.currency, result.price.price)
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
      async function updatePrice() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'catalog.price.update',
            params: {
              id: 1,
              fields: {
                currency: 'USD',
                price: 5000,
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
          console.info(result.price.id, result.price.currency, result.price.price)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updatePrice)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.price.update(
            bitrix_id=1,
            fields={
                "currency": "USD",
                "price": 5000,
            },
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
                'catalog.price.update',
                [
                    'id' => 1,
                    'fields' => [
                        'currency'       => "USD",
                        'price'          => 5000
                    ]
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();

        print_r($result);
    } catch (Throwable $e) {
        echo $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.price.update',
        {
            id: 1,
            fields: {
                currency: "USD",
                price: 5000
            }
        },
        function(result)
        {
            if(result.error())
                console.error(result.error().ex);
            else
                console.log(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'catalog.price.update',
        [
            'id' => 1,
            'fields' => [
                'currency' => 'USD',
                'price' => 5000
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
    res, err := client.Core().Call(ctx, "catalog.price.update", b24.Params{
    	"id": 1,
    	"fields": b24.Params{
    		"currency": "EUR",
    		"price":    5000,
    	},
    })
    if err != nil {
    	return fmt.Errorf("catalog.price.update: %w", err)
    }

    // The method wraps the response in an object with the "price" key.
    raw, ok := b24.Unwrap(res.Result, "price")
    if !ok {
    	return fmt.Errorf("no price key in the response")
    }

    var item struct {
    	CatalogGroupID b24.ID  `json:"catalogGroupId"`
    	Currency       string  `json:"currency"`
    	ID             b24.ID  `json:"id"`
    	Price          float64 `json:"price"`
    	ProductID      b24.ID  `json:"productId"`
    	TimestampX     string  `json:"timestampX"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.CatalogGroupID, item.Currency)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "price": {
            "catalogGroupId": 1,
            "currency": "USD",
            "extraId": null,
            "id": 1,
            "price": 120.75,
            "priceScale": 120.75,
            "productId": 1,
            "quantityFrom": null,
            "quantityTo": null,
            "timestampX": "2024-05-27T12:29:35+02:00"
        }
    },
    "time": {
        "start": 1712327086.69665,
        "finish": 1712327086.95303,
        "duration": 0.256376028060913,
        "processing": 0.0112268924713135,
        "date_start": "2024-05-27T12:29:35+02:00",
        "date_finish": "2024-05-27T12:29:35+02:00",
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
|| **result.price**
[`catalog_price`](../data-types.md#catalog_price) | Object containing information about the product price ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "0",
    "error_description": "Price is not exists"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `200040300020` | Access Denied | No "Edit selling price" access permission ||
|| `400` | `200040300030` | Access Denied | No permission to modify the prices of this product in the catalog information block ||
|| `400` | `100` | Could not find value for parameter {id} | The required parameter `id` is not provided ||
|| `400` | `100` | Could not find value for parameter {fields} | The required parameter `fields` is not provided ||
|| `400` | `0` | Price is not exists | No price with the specified `id` exists ||
|| `400` | `0` | Validate price error. Ranges of base price are not equal to another price group range | `quantityFrom` and `quantityTo` are passed, but the base price of the product has no matching quantity range ||
|| `400` | `BX_ERROR` | Invalid price | A negative number is passed in `price` ||
|| `400` | `BX_ERROR` | Invalid currency | No currency with the code from `currency` exists ||
|| `400` | `BX_ERROR` | Invalid surcharge ID | No markup with the specified `extraId` exists ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-price-add.md)
- [{#T}](./catalog-price-get.md)
- [{#T}](./catalog-price-list.md)
- [{#T}](./catalog-price-delete.md)
- [{#T}](./catalog-price-get-fields.md)
- [{#T}](./catalog-price-modify.md)
