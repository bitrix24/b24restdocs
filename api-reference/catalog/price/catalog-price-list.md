# Retrieve a List of Prices by Filter catalog.price.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View product catalog" access permission

The method `catalog.price.list` returns a list of product prices based on the filter.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select** 
[`array`](../../data-types.md)| An array of fields from [catalog_price](../data-types.md#catalog_price) that need to be selected.

If the array is not provided or an empty array is passed, all available price fields will be selected
||
|| **filter** 
[`object`](../../data-types.md)| An object for filtering selected product prices in `{"field_1": "value_1", ..., "field_N": "value_N"}` format.

Possible values for `field` correspond to the fields of the [catalog_price](../data-types.md#catalog_price) object.

An additional prefix can be assigned to the key to specify the filter behavior. Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN, an array is passed as the value
- `!@` — NOT IN, an array is passed as the value
- `%` — LIKE, substring search. The `%` character does not need to be passed in the filter value. The search looks for a substring in any position of the string
- `=%` — LIKE, substring search. The `%` character must be passed in the value. Examples:
    - `"mol%"` — searches for values starting with "mol"
    - `"%mol"` — searches for values ending with "mol"
    - `"%mol%"` — searches for values where "mol" can be in any position
- `%=` — LIKE (similar to `=%`)
- `!%` — NOT LIKE, substring search. The `%` character does not need to be passed in the filter value. The search goes from both sides
- `!=%` — NOT LIKE, substring search. The `%` character must be passed in the value. Examples:
    - `"mol%"` — searches for values not starting with "mol"
    - `"%mol"` — searches for values not ending with "mol"
    - `"%mol%"` — searches for values where the substring "mol" is not present in any position
- `!%=` — NOT LIKE (similar to `!=%`)
- `=` — equal, exact match (used by default)
- `!=` — not equal
- `!` — not equal
||
|| **order**
[`object`](../../data-types.md)| An object for sorting the selected product prices in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` correspond to the fields of the [catalog_price](../data-types.md#catalog_price) object.

Possible values for `order`:

- `asc` — in ascending order
- `desc` — in descending order
||
|| **start** 
[`integer`](../../data-types.md)| This parameter is used to control pagination.

The page size of results is always static — 50 records.

To select the second page of results, pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` — the number of the desired page
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
    -d '{"select":["id","productId","catalogGroupId","price","currency"],"filter":{"productId":1},"order":{"id":"ASC"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.price.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","productId","catalogGroupId","price","currency"],"filter":{"productId":1},"order":{"id":"ASC"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.price.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type PriceListResult = {
      prices: {
        id: number
        productId: number
        catalogGroupId: number
        price: number
        currency: string
      }[]
    }

    try {
      // catalog.price.list returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<PriceListResult>({
        method: 'catalog.price.list',
        params: {
          select: ['id', 'productId', 'catalogGroupId', 'price', 'currency'],
          filter: { productId: 1 },
          order: { id: 'ASC' },
          start: 0,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Prices count:', result.prices.length, result.prices)
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
      async function fetchPriceList() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // catalog.price.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'catalog.price.list',
            params: {
              select: ['id', 'productId', 'catalogGroupId', 'price', 'currency'],
              filter: { productId: 1 },
              order: { id: 'ASC' },
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
          console.info('Prices count:', result.prices.length, result.prices)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', fetchPriceList)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.price.list(
            select=[
                "id",
                "productId",
                "catalogGroupId",
                "price",
                "currency",
            ],
            filter={
                "productId": 1,
            },
            order={
                "id": "ASC",
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
                'catalog.price.list',
                [
                    'select' => [
                        'id',
                        'productId',
                        'catalogGroupId',
                        'price',
                        'currency'
                    ],
                    'filter' => [
                        'productId' => 1
                    ],
                    'order' => [
                        'id' => 'ASC'
                    ]
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        $result->next();

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching prices: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.price.list',
        {
            select:[
                'id',
                'productId',
                'catalogGroupId',
                'price',
                'currency'
            ],
            filter:{
                'productId': 1
            },
            order:{
                'id': 'ASC'
            }
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.log(result.data());
            result.next();
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'catalog.price.list',
        [
            'select' => [
                'id',
                'productId',
                'catalogGroupId',
                'price',
                'currency'
            ],
            'filter' => [
                'productId' => 1
            ],
            'order' => [
                'id' => 'ASC'
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
    res, err := client.Core().Call(ctx, "catalog.price.list", b24.Params{
    	"select": []string{"id", "productId", "catalogGroupId", "price", "currency"},
    	"filter": b24.Params{
    		"productId": 1,
    	},
    	"order": b24.Params{
    		"id": "ASC",
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.price.list: %w", err)
    }

    // The method wraps the response in an object with the "prices" key.
    raw, ok := b24.Unwrap(res.Result, "prices")
    if !ok {
    	return fmt.Errorf("no prices key in the response")
    }

    var items []struct {
    	CatalogGroupID b24.ID `json:"catalogGroupId"`
    	Currency       string `json:"currency"`
    	ID             b24.ID `json:"id"`
    	Price          int    `json:"price"`
    	ProductID      b24.ID `json:"productId"`
    }
    if err := json.Unmarshal(raw, &items); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, it := range items {
    	fmt.Println(it.CatalogGroupID)
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "prices": [
            {
                "catalogGroupId": 3,
                "currency": "EUR",
                "id": 381,
                "price": 50,
                "productId": 6461
            },
            {
                "catalogGroupId": 1,
                "currency": "USD",
                "id": 787,
                "price": 100.5,
                "productId": 6461
            }
        ]
    },
    "total": 2,
    "time": {
        "start": 1756796975.535775,
        "finish": 1756796975.569925,
        "duration": 0.034150123596191406,
        "processing": 0.0042951107025146484,
        "date_start": "2025-09-02T10:09:35+03:00",
        "date_finish": "2025-09-02T10:09:35+03:00",
        "operating_reset_at": 1756797575,
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
|| **prices**
[`catalog_price[]`](../data-types.md#catalog_price) | An array of objects with information about the selected product prices, structure depends on the `select` parameter ||
|| **total**
[`integer`](../../data-types.md#time) | The total number of records found ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": 200040300010,
    "error_description": "Access Denied"
}
```

{% include notitle [Error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `200040300010` | Access Denied | Insufficient rights to read ||
|| `0` | | Other errors || 
|#

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-price-add.md)
- [{#T}](./catalog-price-update.md)
- [{#T}](./catalog-price-get.md)
- [{#T}](../../../tutorials/catalog/index.md)
- [{#T}](./catalog-price-delete.md)
- [{#T}](./catalog-price-get-fields.md)
- [{#T}](./catalog-price-modify.md)
