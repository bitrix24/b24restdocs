# Retrieve a List of Order Bindings to Sources sale.tradeBinding.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: any user with the "View product catalog" access permission

The method `sale.tradeBinding.list` returns order bindings to sources without the order contents — retrieve the contents with the [sale.order.get](../order/sale-order-get.md) method.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | A list of fields to return. The available fields are listed in the [sale_order_trade_binding](../data-types.md#sale_order_trade_binding) object.

If not provided or an empty array is passed, all fields are returned. Unknown fields are ignored without an error ||
|| **filter**
[`object`](../../data-types.md) | An object for filtering bindings in the format `{"field_1": "value_1", ... "field_N": "value_N"}`. When multiple fields are specified, AND logic is used.

Possible values for `field` correspond to the fields of the [sale_order_trade_binding](../data-types.md#sale_order_trade_binding) object.

An additional prefix can be assigned to the key to clarify the filter behavior. Possible prefix values:
- `=` — equals, exact match, the default prefix
- `!=`, `!` — not equals
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN, the value is passed as an array
- `!@` — NOT IN, the value is passed as an array
- `%` — LIKE, substring search. The `%` character in the filter value does not need to be passed. The substring is searched for in any position of the string
- `=%` — LIKE, substring search. The `%` character needs to be passed in the value. Examples:
    - `mol%` — values starting with "mol"
    - `%mol` — values ending with "mol"
    - `%mol%` — values where "mol" can be in any position
- `%=` — LIKE, substring search. The `%` character needs to be passed in the value, as for `=%`
- `!%` — NOT LIKE, substring search. The `%` character in the filter value does not need to be passed. Returns values that do not contain the substring in any position
- `!=%` — NOT LIKE, substring search. The `%` character needs to be passed in the value. Examples:
    - `mol%` — values not starting with "mol"
    - `%mol` — values not ending with "mol"
    - `%mol%` — values where the substring "mol" is not present in any position
- `!%=` — NOT LIKE, substring search. The `%` character needs to be passed in the value, as for `!=%`

To retrieve the order bindings of one source, filter by `tradingPlatformId` — the identifier from the [sale.tradePlatform.list](../trade-platform/sale-trade-platform-list.md) method.

Unknown fields are ignored without an error. If a field name is misspelled, the method returns all records ||
|| **order**
[`object`](../../data-types.md) | An object for sorting bindings in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` correspond to the fields of the [sale_order_trade_binding](../data-types.md#sale_order_trade_binding) object.

Possible values for `order`:
- `asc` — in ascending order
- `desc` — in descending order

By default, bindings are sorted by `id` in ascending order. Unknown fields are ignored without an error ||
|| **start**
[`integer`](../../data-types.md) | The offset for pagination. The page size is 50 records. The default is `0`, the first page.

For the second page, pass `50`; for the third, `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` is the desired page number.

The value for the next page is returned in the `next` field of the response ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["orderId","tradingPlatformId"],"filter":{"!=tradingPlatformId":10},"order":{"tradingPlatformId":"desc"},"start":0}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.tradeBinding.list
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["orderId","tradingPlatformId"],"filter":{"!=tradingPlatformId":10},"order":{"tradingPlatformId":"desc"},"start":0,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.tradeBinding.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'
    
    declare const $b24: B24Frame
    
    // Shape of the payload returned in result (match the "response handling" section of the page)
    type TradeBindingListResult = {
      tradeBindings: TradeBinding[],
    }
    
    type TradeBinding = {
      orderId: number,
      tradingPlatformId: string,
    }
    
    // sale.tradeBinding.list returns a single page (max 50 records). For the whole result set
    // use a list helper: $b24.actions.v2.callList.make() returns every record as one
    // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
    // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
    // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
    try {
      const response = await $b24.actions.v2.call.make<TradeBindingListResult>({
        method: 'sale.tradeBinding.list',
        params: {
          select: ['orderId', 'tradingPlatformId'],
          filter: { '!=tradingPlatformId': 10 },
          order: { tradingPlatformId: 'desc' },
          start: 0,
        },
        requestId: Text.getUuidRfc4122()
      })
    
      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Trade bindings:', result.tradeBindings, 'Count:', result.tradeBindings.length)
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
      async function listTradeBindings() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()
    
          // sale.tradeBinding.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'sale.tradeBinding.list',
            params: {
              select: ['orderId', 'tradingPlatformId'],
              filter: { '!=tradingPlatformId': 10 },
              order: { tradingPlatformId: 'desc' },
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
          console.info('Trade bindings:', result.tradeBindings, 'Count:', result.tradeBindings.length)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }
    
      document.addEventListener('DOMContentLoaded', listTradeBindings)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.sale.tradebinding.list(
            select=[
                "orderId",
                "tradingPlatformId",
            ],
            filter={
                "!=tradingPlatformId": 10,
            },
            order={
                "tradingPlatformId": "desc",
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
                'sale.tradeBinding.list',
                [
                    'select' => ['orderId', 'tradingPlatformId'],
                    'filter' => ['!=tradingPlatformId' => 10],
                    'order'  => ['tradingPlatformId' => 'desc'],
                    'start'  => 0,
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching trade bindings: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "sale.tradeBinding.list",
        {
            select: ['orderId', 'tradingPlatformId'],
            filter: {'!=tradingPlatformId': 10},
            order: {'tradingPlatformId': 'desc'},
            start: 0
        },
        function(result)
        {
            if(result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.info(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sale.tradeBinding.list',
        [
            'select' => ['orderId', 'tradingPlatformId'],
            'filter' => ['!=tradingPlatformId' => 10],
            'order' => ['tradingPlatformId' => 'desc'],
            'start' => 0
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "sale.tradeBinding.list", b24.Params{
    	"select": []string{"orderId", "tradingPlatformId"},
    	"filter": b24.Params{
    		"!=tradingPlatformId": 10,
    	},
    	"order": b24.Params{
    		"tradingPlatformId": "desc",
    	},
    	"start": 0,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("sale.tradeBinding.list: %w", err)
    }

    // The method wraps the response in an object with the "tradeBindings" key.
    raw, ok := b24.Unwrap(res.Result, "tradeBindings")
    if !ok {
    	return fmt.Errorf("no tradeBindings key in the response")
    }

    var items []struct {
    	OrderID           b24.ID `json:"orderId"`
    	TradingPlatformID b24.ID `json:"tradingPlatformId"`
    }
    if err := json.Unmarshal(raw, &items); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, it := range items {
    	fmt.Println(it.OrderID)
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "tradeBindings": [
            {
                "orderId": 685,
                "tradingPlatformId": "18"
            },
            {
                "orderId": 690,
                "tradingPlatformId": "4"
            },
            {
                "orderId": 607,
                "tradingPlatformId": "3"
            }
        ]
    },
    "total": 3,
    "time": {
        "start": 1712135957.057659,
        "finish": 1712135957.407821,
        "duration": 0.3501620292663574,
        "processing": 0.011919021606445312,
        "date_start": "2024-04-03T11:19:17+02:00",
        "date_finish": "2024-04-03T11:19:17+02:00",
        "operating_reset_at": 1705765533,
        "operating": 3.3076241016387939
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | The root element of the response [(detailed description)](#result) ||
|| **next**
[`integer`](../../data-types.md) | The `start` value for the next page. Returned if more records are found than fit on the current page ||
|| **total**
[`integer`](../../data-types.md) | The total number of records found ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

#### Object result {#result}

#|
|| **Name**
`type` | **Description** ||
|| **tradeBindings**
[`sale_order_trade_binding[]`](../data-types.md#sale_order_trade_binding) | An array of order bindings to sources. The set of fields in each element is defined by the `select` parameter. If nothing is found, the array is empty ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":"200040300010",
    "error_description":"Access Denied"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `200040300010` | Access Denied | Insufficient permissions to execute the method ||
|| `400` | `100` | Invalid order "<VALUE>" | The sort direction in `order` is other than `asc` and `desc` ||
|| `400` | `100` | Order must be a string | The sort direction in `order` is not passed as a string ||
|| — | `0` | — | Other errors, such as fatal errors ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./sale-trade-binding-get-fields.md)
- [{#T}](../trade-platform/sale-trade-platform-list.md)
- [{#T}](../order/sale-order-get.md)

