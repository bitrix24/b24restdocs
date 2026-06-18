# Get a list of orders from sources sale.tradeBinding.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: any user with the "View product catalog" access permission

The method `sale.tradeBinding.list` returns a list of orders from sources.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | An array containing the list of fields to select (see fields of the [sale_order_trade_binding](../data-types.md#sale_order_trade_binding) object).

If not provided or an empty array is passed, all available order fields will be selected. ||
|| **filter**
[`object`](../../data-types.md) | An object for filtering the selected orders in the format `{"field_1": "value_1", ... "field_N": "value_N"}`. When multiple fields are specified, AND logic is used.

Possible values for `field` correspond to the fields of the [sale_order_trade_binding](../data-types.md#sale_order_trade_binding) object.

An additional prefix can be assigned to the key to clarify the filter behavior. Possible prefix values:
- `=` — equals (works with arrays as well)
- `!=` - not equals
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `%` — LIKE, substring search. The `%` character in the filter value does not need to be passed. The search looks for the substring in any position of the string
- `=%` — LIKE, substring search. The `%` character needs to be passed in the value. Examples:
    - "mol%" — searching for values starting with "mol"
    - "%mol" — searching for values ending with "mol"
    - "%mol%" — searching for values where "mol" can be in any position
- `%=` — LIKE (see description above)
- `!%` — NOT LIKE, substring search. The `%` character in the filter value does not need to be passed. The search goes from both sides.
- `!=%` — NOT LIKE, substring search. The `%` character needs to be passed in the value. Examples:
    - "mol%" — searching for values not starting with "mol"
    - "%mol" — searching for values not ending with "mol"
    - "%mol%" — searching for values where the substring "mol" is not present in any position
- `!%=` — NOT LIKE (see description above) ||
|| **order**
[`object`](../../data-types.md) | An object for sorting the selected orders in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` correspond to the fields of the [sale_order_trade_binding](../data-types.md#sale_order_trade_binding) object.

Possible values for `order`:
- `asc` — in ascending order
- `desc` — in descending order ||
|| **start**
[`integer`](../../data-types.md) | This parameter is used for managing pagination.

The page size of results is always static: 50 records.

To select the second page of results, you need to pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` is the desired page number ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```curl
    -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["orderId","tradingPlatformId"],"filter":{"!=tradingPlatformID":10},"order":{"tradingPlatformId":"DESC"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.tradeBinding.list
    ```

- cURL (OAuth)

    ```curl
    -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["orderId","tradingPlatformId"],"filter":{"!=tradingPlatformID":10},"order":{"tradingPlatformId":"DESC"},"auth":"**put_access_token_here**"}' \
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
          filter: { '!=tradingPlatformID': 10 },
          order: { tradingPlatformId: 'DESC' },
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
              filter: { '!=tradingPlatformID': 10 },
              order: { tradingPlatformId: 'DESC' },
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

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'sale.tradeBinding.list',
                [
                    'select' => ['orderId', 'tradingPlatformId'],
                    'filter' => ['!=tradingPlatformID' => 10],
                    'order'  => ['tradingPlatformId' => 'DESC'],
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
        echo 'Error fetching trade bindings: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "sale.tradeBinding.list",
        {
            select: ['orderId', 'tradingPlatformId'],
            filter: {'!=tradingPlatformID': 10},
            order: {'tradingPlatformId': 'DESC'}
        },
        function(result)
        {
            if(result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.dir(result.data());
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
            'filter' => ['!=tradingPlatformID' => 10],
            'order' => ['tradingPlatformId' => 'DESC']
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
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
[`object`](../../data-types.md) | The root element of the response ||
|| **tradeBindings**
[`sale_order_trade_binding[]`](../data-types.md#sale_order_trade_binding) | An array of objects with information about the selected orders ||
|| **total**
[`integer`](../../data-types.md) | The total number of records found ||
|| **time**
[`time`](../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":200040300010,
    "error_description":"Access Denied"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300010` | Insufficient permissions to execute the method ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./sale-trade-binding-get-fields.md)

