# Get a List of Basket Properties sale.basketproperties.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: store manager

This method returns a set of properties for the items (positions) in the basket, selected based on a filter.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | An array containing the list of fields to be selected (see fields of the object [`sale_basket_item_property`](../data-types.md#sale_basket_item_property)).

If the array is not provided or an empty array is passed, all available fields of the basket item properties will be selected. ||
|| **filter**
[`object`](../../data-types.md) | An object for filtering the selected catalog sections in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

Possible values for `field` correspond to the fields of the object [`sale_basket_item_property`](../data-types.md#sale_basket_item_property).

An additional prefix can be specified for the key to clarify the filter behavior. Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN (an array is passed as the value)
- `!@` — NOT IN (an array is passed as the value)
- `%` — LIKE, substring search. The % symbol in the filter value does not need to be passed. The search looks for the substring in any position of the string.
- `=%` — LIKE, substring search. The % symbol needs to be passed in the value. Examples:
  - `"mol%"` — searching for values starting with "mol"
  - `"%mol"` — searching for values ending with "mol"
  - `"%mol%"` — searching for values where "mol" can be in any position
- `%=` — LIKE (see description above)
- `!%` — NOT LIKE, substring search. The % symbol in the filter value does not need to be passed. The search goes from both sides.
- `!=%` — NOT LIKE, substring search. The % symbol needs to be passed in the value. Examples:
  - `"mol%"` — searching for values not starting with "mol"
  - `"%mol"` — searching for values not ending with "mol"
  - `"%mol%"` — searching for values where the substring "mol" is not present in any position
- `!%=` – NOT LIKE (see description above)
- `=` exact match (used by default)
- `!=` — not equal
- `!` — not equal
||
|| **order**
[`object`](../../data-types.md) | An object for sorting the selected groups of properties in the format `{"field_1": "order_1", ... "field_N": "order_N"}`. 
Possible values for `field` correspond to the fields of the object [`sale_basket_item_property`](../data-types.md#sale_basket_item_property).

Possible values for `order`:
- `asc` — in ascending order
- `desc` — in descending order
||
|| **start**
[`integer`](../../data-types.md) | This parameter is used to manage pagination.
The page size of results is always static - 50 records.
To select the second page of results, you need to pass the value - 50. To select the third page of results, the value - 100, and so on.
The formula for calculating the start parameter value:
start = (N-1) * 50, where N is the desired page number
||
|#

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","basketId","value","name","code"],"filter":{"basketId":6806},"order":{"id":"desc"},"start":0}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.basketproperties.list
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","basketId","value","name","code"],"filter":{"basketId":6806},"order":{"id":"desc"},"start":0,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.basketproperties.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type BasketPropertiesListResult = {
      basketProperties: {
        id: number
        basketId: number
        name: string
        code: string
        value: string
      }[]
    }

    // sale.basketproperties.list returns a single page (max 50 records). For the whole result set
    // use a list helper: $b24.actions.v2.callList.make() returns every record as one
    // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
    // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
    // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
    try {
      const response = await $b24.actions.v2.call.make<BasketPropertiesListResult>({
        method: 'sale.basketproperties.list',
        params: {
          select: [
            'id',
            'basketId',
            'value',
            'name',
            'code',
          ],
          filter: { basketId: 6806 },
          order: { id: 'desc' },
          start: 0,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Basket properties count:', result.basketProperties.length)
        console.info('First basket property:', result.basketProperties[0])
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
      async function getBasketPropertiesList() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // sale.basketproperties.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'sale.basketproperties.list',
            params: {
              select: [
                'id',
                'basketId',
                'value',
                'name',
                'code',
              ],
              filter: { basketId: 6806 },
              order: { id: 'desc' },
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
          console.info('Basket properties count:', result.basketProperties.length)
          console.info('First basket property:', result.basketProperties[0])
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getBasketPropertiesList)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'sale.basketproperties.list',
                [
                    'select' => [
                        'id',
                        'basketId',
                        'value',
                        'name',
                        'code',
                    ],
                    'filter' => [
                        'basketId' => 6806,
                    ],
                    'order' => [
                        'id' => 'desc',
                    ],
                    'start' => 0,
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your data processing logic here
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "sale.basketproperties.list",
        {
            select: [
                'id',
                'basketId',
                'value',
                'name',
                'code',
            ],
            filter: {
                'basketId': 6806,
            },
            order: {
                id: 'desc',
            },
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
                    console.log(result.data);

                    if (result.more())
                    {
                        result.next();
                    }
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
        'sale.basketproperties.list',
        [
            'select' => ['id', 'basketId', 'value', 'name', 'code'],
            'filter' => ['basketId' => 6806],
            'order' => ['id' => 'desc'],
            'start' => 0
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "basketProperties": [
            {
                "basketId": 6806,
                "code": "PARTNUM",
                "id": 18,
                "name": "Batch Number",
                "value": "F45TK266"
            },
            {
                "basketId": 6806,
                "code": "ARTICUL",
                "id": 17,
                "name": "Article",
                "value": "123-456-789"
            }
        ]
    },
    "total": 2,
    "time": {
        "start": 1714052872.323321,
        "finish": 1714052872.818371,
        "duration": 0.49504995346069336,
        "processing": 0.0272219181060791,
        "date_start": "2024-04-25T15:47:52+02:00",
        "date_finish": "2024-04-25T15:47:52+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | The root element of the response ||
|| **basketProperties**
[`sale_basket_item_property[]`](../data-types.md#sale_basket_item_property) | An array of objects containing information about the selected properties of items (positions) in the basket for orders ||
|| **total**
[`integer`](../../data-types.md) | The total number of records found ||
|| **time**
[`time`](../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error":0,
    "error_description":"error"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300010` | Insufficient permissions to read  ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./sale-basket-properties-add.md)
- [{#T}](./sale-basket-properties-update.md)
- [{#T}](./sale-basket-properties-get.md)
- [{#T}](./sale-basket-properties-delete.md)
- [{#T}](./sale-basket-properties-get-fields.md)
