# Get a List of Inventory Balances by Filter v 3.0 catalog.store.product.list

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

The `catalog.store.product.list` method returns product inventory balances in warehouses. Each record shows the inventory balance of one product in one warehouse.

To find the inventory balances of a product in all warehouses, filter the list by `productId`. To retrieve all products in a warehouse, filter by `storeId`.

The `catalog.store.product.*` methods only read inventory balances. To change inventory balances, use inventory management documents in the [catalog.document.*](../document/index.md) methods. For example, a processed stock receipt document increases the inventory balance in a warehouse, and a write-off document decreases it.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
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
|| **filter**
[`array`](../../data-types.md) | List filter in the REST 3.0 format: an array of conditions like `["field", "operator", "value"]`, for example `[["productId", "=", 6595]]`. The filter syntax is described in the [REST 3.0 overview](../../rest-v3.md#filter).

You can filter by all fields from the `select` parameter. The product identifier can be retrieved using the [catalog.product.list](../product/catalog-product-list-rest-v3.md) method, and the warehouse identifier using the [catalog.store.list](../store/catalog-store-list-rest-v3.md) method.

The `=`, `!=`, `>`, `>=`, `<`, `<=`, `in`, and `between` operators are supported. An empty array in `in` returns an empty list. Groups with `or` logic and negation are not supported. Instead of `or`, use the `in` operator or run several requests; instead of negation, use the `!=` operator ||
|| **order**
[`object`](../../data-types.md) | Sorting in the `{"field": "direction"}` format, for example `{"storeId": "asc"}`.

You can sort by all fields from the `select` parameter.

Possible direction values:

- `asc` — ascending
- `desc` — descending ||
|| **pagination**
[`object`](../../data-types.md) | Pagination parameters. [Object structure description](#pagination) ||
|#

### Parameter pagination {#pagination}

#|
|| **Name**
`type` | **Description** ||
|| **limit**
[`integer`](../../data-types.md) | Number of records per page. Values greater than `1000` are reduced to `1000`. Zero, negative, and non-numeric values cause an error.

Default: `50` ||
|| **page**
[`integer`](../../data-types.md) | Page number, starting from `1`. The method skips `(page - 1) * limit` records and ignores `offset` ||
|| **offset**
[`integer`](../../data-types.md) | Number of records to skip from the beginning of the list.

Default: `0` ||
|#

To export all inventory balances, pass `{"id": "asc"}` in `order`, and in each subsequent request add the `["id", ">", N]` filter condition, where N is the last `id` of the previous page. This works better than increasing `offset`: with a large `offset`, the database scans all skipped records.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.store.product.list`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","productId","storeId","amount","quantityReserved"],"filter":[["productId","=",6595],["amount",">",0]],"order":{"storeId":"asc"},"pagination":{"limit":10}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.store.product.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","productId","storeId","amount","quantityReserved"],"filter":[["productId","=",6595],["amount",">",0]],"order":{"storeId":"asc"},"pagination":{"limit":10},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.store.product.list
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type StoreProductListResult = {
      items: {
        id: number
        productId: number
        storeId: number
        amount: number
        quantityReserved: number
      }[]
    }

    try {
      const response = await $b24.actions.v3.call.make<StoreProductListResult>({
        method: 'catalog.store.product.list',
        params: {
          select: ['id', 'productId', 'storeId', 'amount', 'quantityReserved'],
          filter: [
            ['productId', '=', 6595],
            ['amount', '>', 0],
          ],
          order: {
            storeId: 'asc',
          },
          pagination: {
            limit: 10,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        result.items.forEach(stock => console.info(stock.storeId, stock.amount))
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
      async function listStoreProducts() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.store.product.list',
            params: {
              select: ['id', 'productId', 'storeId', 'amount', 'quantityReserved'],
              filter: [
                ['productId', '=', 6595],
                ['amount', '>', 0],
              ],
              order: {
                storeId: 'asc',
              },
              pagination: {
                limit: 10,
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          result.items.forEach(stock => console.info(stock.storeId, stock.amount))
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listStoreProducts)
    </script>
    ```

- Python

    The `catalog.store.product.*` REST 3.0 methods have no ready-made wrappers in `b24pysdk`, and the SDK sends to REST 3.0 only the methods listed in `Config().api_v3_methods`. Add the method to this list and call it with `call_method` of the client token, passing `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "catalog.store.product.list"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})
    bitrix_token = client.get_token()

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "select": ["id", "productId", "storeId", "amount", "quantityReserved"],
                "filter": [
                    ["productId", "=", 6595],
                    ["amount", ">", 0],
                ],
                "order": {
                    "storeId": "asc",
                },
                "pagination": {
                    "limit": 10,
                },
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
                'catalog.store.product.list',
                [
                    'select' => ['id', 'productId', 'storeId', 'amount', 'quantityReserved'],
                    'filter' => [
                        ['productId', '=', 6595],
                        ['amount', '>', 0],
                    ],
                    'order' => [
                        'storeId' => 'asc',
                    ],
                    'pagination' => [
                        'limit' => 10,
                    ],
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
        'catalog.store.product.list',
        {
            select: ['id', 'productId', 'storeId', 'amount', 'quantityReserved'],
            filter: [
                ['productId', '=', 6595],
                ['amount', '>', 0],
            ],
            order: {
                storeId: 'asc',
            },
            pagination: {
                limit: 10,
            },
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
        'catalog.store.product.list',
        [
            'select' => ['id', 'productId', 'storeId', 'amount', 'quantityReserved'],
            'filter' => [
                ['productId', '=', 6595],
                ['amount', '>', 0],
            ],
            'order' => [
                'storeId' => 'asc',
            ],
            'pagination' => [
                'limit' => 10,
            ],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "catalog.store.product.list", b24.Params{
    	"select": []string{"id", "productId", "storeId", "amount", "quantityReserved"},
    	"filter": []any{
    		[]any{"productId", "=", 6595},
    		[]any{"amount", ">", 0},
    	},
    	"order": b24.Params{
    		"storeId": "asc",
    	},
    	"pagination": b24.Params{
    		"limit": 10,
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.store.product.list: %w", err)
    }

    var result struct {
    	Items []struct {
    		ID               int     `json:"id"`
    		ProductID        int     `json:"productId"`
    		StoreID          int     `json:"storeId"`
    		Amount           float64 `json:"amount"`
    		QuantityReserved float64 `json:"quantityReserved"`
    	} `json:"items"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, stock := range result.Items {
    	fmt.Println(stock.StoreID, stock.Amount, stock.QuantityReserved)
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "items": [
            {
                "id": 3,
                "productId": 6595,
                "storeId": 1,
                "amount": 15,
                "quantityReserved": 30
            },
            {
                "id": 1,
                "productId": 6595,
                "storeId": 11,
                "amount": 10,
                "quantityReserved": 0
            }
        ]
    },
    "time": {
        "start": 1794922635,
        "finish": 1794922635.044875,
        "duration": 0.04487490653991699,
        "processing": 0,
        "date_start": "2026-11-17T16:37:15+02:00",
        "date_finish": "2026-11-17T16:37:15+02:00",
        "operating_reset_at": 1794922920,
        "operating": 0.41770195960998535
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Object with the response data ||
|| **result.items**
[`array`](../../data-types.md) | Array of inventory balance records on the page. The set of fields depends on `select`. [Field description](#items) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

The method does not return the total number of records. If the response contains fewer records than `limit`, the page is the last one.

#### Items Element {#items}

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
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION",
        "message": "Error validating request object.",
        "validation": [
            {
                "message": "Filter OR groups are not supported"
            }
        ]
    }
}
```

{% include notitle [error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Filter OR groups are not supported | Remove the group with `or` logic from the filter. Make several requests or use the `in` operator ||
|| `-` | Filter NOT groups are not supported | Remove the negation from the filter. Use the `!=` operator ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select`, `filter`, `order` | Unknown field `#FIELD#` found in entity `StoreProductDto` | Specify only the fields from the [items element description](#items) ||
|#

#### Filter Format Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDFILTEREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter` | Invalid filter `Unknown filter condition` | Pass `filter` as an array of conditions, for example `[["productId","=",6595]]` ||
|#

#### Filter Operator Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNFILTEROPERATOREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter` | Unknown filter operator `#OPERATOR#` | Use the `=`, `!=`, `>`, `>=`, `<`, `<=`, `in`, `between` operators ||
|#

#### Sorting Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDORDEREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `order` | Invalid sort order `#ORDER#` | Pass the `asc` or `desc` direction ||
|#

#### Pagination Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `pagination` | Incorrect `pagination` parameter value: `#PAGINATION#` | Pass `limit` as a positive number, `offset` as a non-negative number, and `page` as a number ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | The user does not have the "View product catalog" or "View and select warehouses" access permission ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-store-product-get-rest-v3.md)
- [{#T}](./catalog-store-product-field-list.md)
- [{#T}](../store/catalog-store-list-rest-v3.md)
- [{#T}](../document/index.md)
