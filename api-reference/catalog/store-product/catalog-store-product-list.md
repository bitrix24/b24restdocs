# Get a list of inventory balances by filter catalog.storeproduct.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View product catalog" or "View and select warehouses" access permission

The method `catalog.storeproduct.list` returns product inventory balances by warehouse with filtering, sorting, and pagination.

One record is the balance of one product in one warehouse. The response includes only records of warehouses available to the user under the "View and select warehouses" permission.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | An array of inventory balance record fields to return. Available fields: `id`, `productId`, `storeId`, `amount`, `quantityReserved`. The fields are described in the [catalog_storeproduct](../data-types.md#catalog_storeproduct) object.

If the array is not passed or is empty, the method returns all fields. The method skips unknown fields without an error. Write field names in camelCase: if `select` contains no known field, for example `["ID", "AMOUNT"]`, the method returns all fields
||
|| **filter**
[`object`](../../data-types.md) | An object for filtering inventory balance records in the format `{"field_1": "value_1", ..., "field_N": "value_N"}`.

Possible values for `field` are the fields listed in `select`. For example, `{"productId": 6595}` returns the product balances in all warehouses, and `{"storeId": 1}` returns the balances of all products in the warehouse.

An additional prefix can be assigned to the key to specify the filter behavior. Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN, an array is passed as the value
- `!@` — NOT IN, an array is passed as the value
- `=` — equal, exact match (used by default)
- `!=` — not equal
- `!` — not equal

An array without a prefix works as IN: `{"productId": [6973, 6595]}` returns the balances of both products. To find records without a balance, pass `{"amount": null}`.

The `<` and `<=` conditions on `amount` and `quantityReserved` also match records where the value is `null`. To get only records with a filled-in balance, add the `">amount": 0` or `">=amount": 0` condition.

The substring search prefixes `%`, `=%`, and `!%` do not apply to these fields: all fields are numeric, and with these prefixes the method finds no records.

The method skips unknown fields and uppercase names, for example `PRODUCT_ID`, without an error and returns records without applying that condition. Check field names against the list above
||
|| **order**
[`object`](../../data-types.md) | An object for sorting inventory balance records in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` are the fields listed in `select`.

Possible values for `order`:

- `asc` — in ascending order
- `desc` — in descending order

The direction is case-insensitive: `DESC` and `desc` work the same way. The method skips unknown fields in `order` without an error
||
|| **start**
[`integer`](../../data-types.md) | This parameter is used to manage pagination.

The page size of results is always static — 50 records.

To select the second page of results, pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` — the desired page number.

If `start` exceeds the number of records found, the method returns an empty `storeProducts` array
||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

The example retrieves the warehouses that have the product with the `6595` identifier in stock, from the largest balance to the smallest.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","productId","storeId","amount","quantityReserved"],"filter":{"productId":6595,">amount":0},"order":{"amount":"desc"},"start":0}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.storeproduct.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","productId","storeId","amount","quantityReserved"],"filter":{"productId":6595,">amount":0},"order":{"amount":"desc"},"start":0,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.storeproduct.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type StoreProductListResult = {
      storeProducts: {
        id: number
        productId: number
        storeId: number
        amount: number | null
        quantityReserved: number | null
      }[]
    }

    try {
      // catalog.storeproduct.list returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<StoreProductListResult>({
        method: 'catalog.storeproduct.list',
        params: {
          select: ['id', 'productId', 'storeId', 'amount', 'quantityReserved'],
          filter: {
            productId: 6595,
            '>amount': 0,
          },
          order: {
            amount: 'desc',
          },
          start: 0,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        for (const row of result.storeProducts) {
          console.info('Store', row.storeId, 'amount', row.amount)
        }
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
      async function fetchStoreProducts() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'catalog.storeproduct.list',
            params: {
              select: ['id', 'productId', 'storeId', 'amount', 'quantityReserved'],
              filter: {
                productId: 6595,
                '>amount': 0,
              },
              order: {
                amount: 'desc',
              },
              start: 0,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          for (const row of result.storeProducts) {
            console.info('Store', row.storeId, 'amount', row.amount)
          }
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', fetchStoreProducts)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.storeproduct.list(
            select=["id", "productId", "storeId", "amount", "quantityReserved"],
            filter={
                "productId": 6595,
                ">amount": 0,
            },
            order={
                "amount": "desc",
            },
            start=0,
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
                'catalog.storeproduct.list',
                [
                    'select' => ['id', 'productId', 'storeId', 'amount', 'quantityReserved'],
                    'filter' => [
                        'productId' => 6595,
                        '>amount'   => 0,
                    ],
                    'order'  => [
                        'amount' => 'desc',
                    ],
                    'start'  => 0,
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
        'catalog.storeproduct.list',
        {
            select: ['id', 'productId', 'storeId', 'amount', 'quantityReserved'],
            filter: {
                productId: 6595,
                '>amount': 0,
            },
            order: {
                amount: 'desc',
            },
            start: 0,
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
        'catalog.storeproduct.list',
        [
            'select' => ['id', 'productId', 'storeId', 'amount', 'quantityReserved'],
            'filter' => [
                'productId' => 6595,
                '>amount' => 0,
            ],
            'order' => [
                'amount' => 'desc',
            ],
            'start' => 0,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "catalog.storeproduct.list", b24.Params{
    	"select": []string{"id", "productId", "storeId", "amount", "quantityReserved"},
    	"filter": b24.Params{
    		"productId": 6595,
    		">amount":   0,
    	},
    	"order": b24.Params{
    		"amount": "desc",
    	},
    	"start": 0,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.storeproduct.list: %w", err)
    }

    // The method wraps the response in an object with the "storeProducts" key.
    raw, ok := b24.Unwrap(res.Result, "storeProducts")
    if !ok {
    	return fmt.Errorf("no storeProducts key in the response")
    }

    // amount and quantityReserved can be null, so pointers are used for them.
    var items []struct {
    	ID               b24.ID   `json:"id"`
    	ProductID        b24.ID   `json:"productId"`
    	StoreID          b24.ID   `json:"storeId"`
    	Amount           *float64 `json:"amount"`
    	QuantityReserved *float64 `json:"quantityReserved"`
    }
    if err := json.Unmarshal(raw, &items); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, it := range items {
    	fmt.Println(it.StoreID, it.Amount)
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "storeProducts": [
            {
                "amount": 15,
                "id": 3,
                "productId": 6595,
                "quantityReserved": 30,
                "storeId": 1
            },
            {
                "amount": 10,
                "id": 1,
                "productId": 6595,
                "quantityReserved": null,
                "storeId": 11
            }
        ]
    },
    "total": 2,
    "time": {
        "start": 1793874773,
        "finish": 1793874773.615197,
        "duration": 0.615196943283081,
        "processing": 0,
        "date_start": "2026-11-05T13:32:53+03:00",
        "date_finish": "2026-11-05T13:32:53+03:00",
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
|| **result.storeProducts**
[`catalog_storeproduct[]`](../data-types.md#catalog_storeproduct) | An array of inventory balance records. Each record contains only the fields from `select`. Empty `amount` and `quantityReserved` values are returned as `null` ||
|| **total**
[`integer`](../../data-types.md) | Total number of records found ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "100",
    "error_description": "Invalid order \"BAD\""
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `200040300010` | Access Denied | The user has neither the "View product catalog" nor the "View and select warehouses" permission ||
|| `400` | `100` | Invalid order "BAD" | A direction other than `asc` and `desc` is passed in `order`. The error text shows the direction in uppercase ||
|| `400` | `100` | Invalid value {abc} to match with parameter {filter}. Should be value of type array. | `filter` is not an object ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-store-product-get.md)
- [{#T}](./catalog-store-product-get-fields.md)
