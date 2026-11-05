# Get Product Inventory by ID catalog.storeproduct.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View product catalog" or "View and select warehouses" access permission

The method `catalog.storeproduct.get` returns the product quantity and reserve in a warehouse by the inventory balance record identifier.

The method returns a record only if its warehouse is available to the user under the "View and select warehouses" permission. For a record of an unavailable warehouse, the method returns a "not found" error.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`catalog_storeproduct.id`](../data-types.md#catalog_storeproduct) | Inventory balance record identifier, not the product identifier.

You can retrieve the record identifier by product and warehouse using the [catalog.storeproduct.list](./catalog-store-product-list.md) method with the `{"productId": ..., "storeId": ...}` filter ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":3}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.storeproduct.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":3,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.storeproduct.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type StoreProductGetResult = {
      storeProduct: {
        amount: number | null
        id: number
        productId: number
        quantityReserved: number | null
        storeId: number
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<StoreProductGetResult>({
        method: 'catalog.storeproduct.get',
        params: {
          id: 3,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Amount:', result.storeProduct.amount, 'Reserved:', result.storeProduct.quantityReserved)
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
      async function getStoreProduct() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'catalog.storeproduct.get',
            params: {
              id: 3,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Amount:', result.storeProduct.amount, 'Reserved:', result.storeProduct.quantityReserved)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getStoreProduct)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.storeproduct.get(
            bitrix_id=3,
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
                'catalog.storeproduct.get',
                [
                    'id' => 3,
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
        'catalog.storeproduct.get',
        {
            id: 3,
        },
        function(result)
        {
            if (result.error())
                console.error(result.error());
            else
                console.log(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'catalog.storeproduct.get',
        [
            'id' => 3,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "catalog.storeproduct.get", b24.Params{
    	"id": 3,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.storeproduct.get: %w", err)
    }

    // The method wraps the response in an object with the "storeProduct" key.
    raw, ok := b24.Unwrap(res.Result, "storeProduct")
    if !ok {
    	return fmt.Errorf("no storeProduct key in the response")
    }

    // amount and quantityReserved can be null, so pointers are used for them.
    var item struct {
    	ID               b24.ID   `json:"id"`
    	ProductID        b24.ID   `json:"productId"`
    	StoreID          b24.ID   `json:"storeId"`
    	Amount           *float64 `json:"amount"`
    	QuantityReserved *float64 `json:"quantityReserved"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ID, item.StoreID)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "storeProduct": {
            "amount": 15,
            "id": 3,
            "productId": 6595,
            "quantityReserved": 30,
            "storeId": 1
        }
    },
    "time": {
        "start": 1793874771,
        "finish": 1793874771.975931,
        "duration": 0.97593092918396,
        "processing": 0,
        "date_start": "2026-11-05T13:32:51+03:00",
        "date_finish": "2026-11-05T13:32:51+03:00",
        "operating_reset_at": 1793875320,
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
|| **result.storeProduct**
[`catalog_storeproduct`](../data-types.md#catalog_storeproduct) | Inventory balance record: product, warehouse, quantity in the warehouse, and reserve. If the quantity or reserve is not filled in, `amount` and `quantityReserved` are returned as `null` ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "",
    "error_description": "storeProduct does not exist."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `200040300010` | Access Denied | The user has neither the "View product catalog" nor the "View and select warehouses" permission ||
|| `400` | Empty value | storeProduct does not exist. | There is no inventory balance record with this `id`, or its warehouse is not available to the user. The method returns the same error if `id` is a string that is not a number ||
|| `400` | `100` | Could not find value for parameter {id} | The `id` parameter is not passed ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-store-product-list.md)
- [{#T}](./catalog-store-product-get-fields.md)
