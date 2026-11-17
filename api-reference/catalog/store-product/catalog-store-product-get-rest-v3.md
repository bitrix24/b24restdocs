# Get Product Inventory Balance in a Warehouse by ID v 3.0 catalog.store.product.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View product catalog" or "View and select warehouses" access permission

{% note info "" %}

The method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The `catalog.store.product.get` method returns an inventory balance record: how many units of a product are in one warehouse and how many are reserved.

The method takes the inventory balance record identifier, not the product identifier. To retrieve the inventory balances of a product across all warehouses, use the [catalog.store.product.list](./catalog-store-product-list-rest-v3.md) method with a filter by `productId`.

The `catalog.store.product.*` methods only read inventory balances. To change inventory balances, use inventory management documents in the [catalog.document.*](../document/index.md) methods. For example, a processed stock receipt document increases the inventory balance in a warehouse, and a write-off document decreases it.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Inventory balance record identifier.

The identifier can be retrieved using the [catalog.store.product.list](./catalog-store-product-list-rest-v3.md) method ||
|| **select**
[`array`](../../data-types.md) | Array of inventory balance record fields to return in the response.

Available fields:

- `id` — inventory balance record identifier
- `productId` — product or variation identifier
- `storeId` — warehouse identifier
- `amount` — product quantity in the warehouse
- `quantityReserved` — reserved product quantity

The `id` identifier is always returned. The value `["*"]` is not supported.

Default: all fields ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.store.product.get`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":3}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.store.product.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":3,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.store.product.get
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type StoreProductGetResult = {
      item: {
        id: number
        productId: number
        storeId: number
        amount: number
        quantityReserved: number
      }
    }

    try {
      const response = await $b24.actions.v3.call.make<StoreProductGetResult>({
        method: 'catalog.store.product.get',
        params: {
          id: 3,
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.item.storeId, result.item.amount)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function getStoreProduct() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.store.product.get',
            params: {
              id: 3,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.item.storeId, result.item.amount)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getStoreProduct)
    </script>
    ```

- Python

    The `catalog.store.product.*` REST 3.0 methods have no ready-made wrappers in `b24pysdk`, and the SDK sends to REST 3.0 only the methods listed in `Config().api_v3_methods`. Add the method to this list and call it with `call_method` of the client token, passing `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "catalog.store.product.get"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})
    bitrix_token = client.get_token()

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "id": 3,
            },
            prefer_version=3,
        )
        result = response["result"]
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"code: {error.code}",
            f"message: {error.error.message}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

- PHP

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'catalog.store.product.get',
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```js
    BX24.callMethod(
        'catalog.store.product.get',
        {
            id: 3,
        },
        result => {
            if (result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP CRest

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'catalog.store.product.get',
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
    res, err := client.Core().Call(ctx, "catalog.store.product.get", b24.Params{
    	"id": 3,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.store.product.get: %w", err)
    }

    // The method wraps the response in an object with the "item" key.
    raw, ok := b24.Unwrap(res.Result, "item")
    if !ok {
    	return fmt.Errorf("the response has no item key")
    }

    var item struct {
    	ID               int     `json:"id"`
    	ProductID        int     `json:"productId"`
    	StoreID          int     `json:"storeId"`
    	Amount           float64 `json:"amount"`
    	QuantityReserved float64 `json:"quantityReserved"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ProductID, item.StoreID, item.Amount)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "id": 3,
            "productId": 6595,
            "storeId": 1,
            "amount": 15,
            "quantityReserved": 30
        }
    },
    "time": {
        "start": 1794922634,
        "finish": 1794922634.562411,
        "duration": 0.5624110698699951,
        "processing": 0,
        "date_start": "2026-11-17T16:37:14+02:00",
        "date_finish": "2026-11-17T16:37:14+02:00",
        "operating_reset_at": 1794923220,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Object with the response data ||
|| **result.item**
[`object`](../../data-types.md) | Inventory balance record. The set of fields depends on `select`. [Field description](#item) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Item Object {#item}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Inventory balance record identifier ||
|| **productId**
[`integer`](../../data-types.md) | Product or variation identifier ||
|| **storeId**
[`integer`](../../data-types.md) | Warehouse identifier ||
|| **amount**
[`double`](../../data-types.md) | Product quantity in the warehouse ||
|| **quantityReserved**
[`double`](../../data-types.md) | Reserved product quantity. Can be greater than `amount` ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION",
        "message": "Entity with ID 999 was not found."
    }
}
```

{% include notitle [error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Required field `id` is missing | Pass the inventory balance record `id` as a positive integer ||
|#

#### Object Not Found Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Entity with ID `#ID#` was not found | There is no inventory balance record with this `id`. Pass the inventory balance record identifier from the [catalog.store.product.list](./catalog-store-product-list-rest-v3.md) method, not the product identifier ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | Unknown field `#FIELD#` found in entity `StoreProductDto` | Specify only the fields from the [item object description](#item) ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | The user does not have the "View product catalog" or "View and select warehouses" access permission ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-store-product-list-rest-v3.md)
- [{#T}](./catalog-store-product-field-list.md)
- [{#T}](../store/catalog-store-list-rest-v3.md)
