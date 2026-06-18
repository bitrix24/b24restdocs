# Get a List of Items (Positions) in the Cart sale.basketitem.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: store manager

This method returns a set of items (positions) in the cart filtered by the specified criteria.

## Method Parameters

#| 
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | 
An array of fields to be selected (see fields of the [sale_basket_item](../data-types.md) object).

If the array is not provided or is empty, all available fields from the trade catalogs will be selected.
||
|| **filter**
[`object`](../../data-types.md) | An object for filtering the selected records in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

Possible values for `field` correspond to the fields of the [sale_basket_item](../data-types.md) object.

An additional prefix can be specified for the key to clarify the filter's behavior. Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN, an array is passed as the value
- `!@` — NOT IN, an array is passed as the value
- `%` — LIKE, substring search. The `%` character does not need to be passed in the filter value. The search looks for the substring in any position of the string.
- `=%` — LIKE, substring search. The `%` character must be passed in the value. Examples:
    - `"mol%"` — searches for values starting with "mol"
    - `"%mol"` — searches for values ending with "mol"
    - `"%mol%"` — searches for values where "mol" can be in any position
- `%=` — LIKE (similar to `=%`)
- `!%` — NOT LIKE, substring search. The `%` character does not need to be passed in the filter value. The search goes from both sides.
- `!=%` — NOT LIKE, substring search. The `%` character must be passed in the value. Examples:
    - `"mol%"` — searches for values not starting with "mol"
    - `"%mol"` — searches for values not ending with "mol"
    - `"%mol%"` — searches for values where the substring "mol" is not present in any position
- `!%=` — NOT LIKE (similar to `!=%`)
- `=` — equal, exact match (used by default)
- `!=` — not equal
- `!` — not equal ||
|| **order**
[`object`](../../data-types.md) | 
An object for sorting the selected records in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` correspond to the fields of the [sale_basket_item](../data-types.md) object.

Possible values for `order`:
- `asc` — in ascending order
- `desc` — in descending order
||
|| **start**
[`integer`](../../data-types.md) | This parameter is used to manage pagination.

The page size of results is always static — 50 records.

To select the second page of results, pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` is the desired page number
||
|#

## Code Examples

{% include [Examples Note](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","orderId","productId","name","price","currency"],"filter":{"@orderId":[5147,5146]},"order":{"id":"desc"},"start":0}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.basketitem.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","orderId","productId","name","price","currency"],"filter":{"@orderId":[5147,5146]},"order":{"id":"desc"},"start":0,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.basketitem.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type BasketItemListResult = {
      basketItems: {
        id: number
        orderId: number
        productId: number
        name: string
        price: number
        currency: string
      }[]
    }

    try {
      // sale.basketitem.list returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<BasketItemListResult>({
        method: 'sale.basketitem.list',
        params: {
          select: [
            'id',
            'orderId',
            'productId',
            'name',
            'price',
            'currency',
          ],
          filter: {
            '@orderId': [5147, 5146],
          },
          order: {
            id: 'desc',
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
        console.info('Basket items:', result.basketItems, 'Count:', result.basketItems.length)
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
      async function listBasketItems() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // sale.basketitem.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'sale.basketitem.list',
            params: {
              select: [
                'id',
                'orderId',
                'productId',
                'name',
                'price',
                'currency',
              ],
              filter: {
                '@orderId': [5147, 5146],
              },
              order: {
                id: 'desc',
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
          console.info('Basket items:', result.basketItems, 'Count:', result.basketItems.length)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listBasketItems)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'sale.basketitem.list',
                [
                    'select' => [
                        'id',
                        'orderId',
                        'productId',
                        'name',
                        'price',
                        'currency',
                    ],
                    'filter' => [
                        '@orderId' => [5147, 5146],
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
        // Your data processing logic
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "sale.basketitem.list",
        {
            select: [
                'id',
                'orderId',
                'productId',
                'name',
                'price',
                'currency',
            ],
            filter: {
                '@orderId': [5147, 5146],
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
        'sale.basketitem.list',
        [
            'select' => [
                'id',
                'orderId',
                'productId',
                'name',
                'price',
                'currency',
            ],
            'filter' => [
                '@orderId' => [5147, 5146],
            ],
            'order' => [
                'id' => 'desc',
            ],
            'start' => 0,
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
        "basketItems": [
            {
                "currency": "USD",
                "id": 6802,
                "name": "Office Chair",
                "orderId": 5147,
                "price": 1100,
                "productId": 4343
            },
            
            {
                "currency": "USD",
                "id": 6791,
                "name": "Catalog Chair",
                "orderId": 5146,
                "price": 900,
                "productId": 0
            },
            {
                "currency": "USD",
                "id": 6770,
                "name": "Assembly",
                "orderId": 5146,
                "price": 1110,
                "productId": 4342
            }
        ]
    },
    "total": 3,
    "time": {
        "start": 1713958546.058793,
        "finish": 1713958548.507179,
        "duration": 2.4483859539031982,
        "processing": 0.2580289840698242,
        "date_start": "2024-04-24T13:35:46+02:00",
        "date_finish": "2024-04-24T13:35:48+02:00",
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
|| **basketItems**
[`sale_basket_item[]`](../data-types.md) | An array of objects containing information about the selected items (positions) in the order cart ||
|| **total**
[`integer`](../../data-types.md) | Total number of records found ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
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
|| `200040300010` | Insufficient permissions to read
|| 
|| `0` | Other errors (e.g., fatal errors)
|| 
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sale-basket-item-add.md)
- [{#T}](./sale-basket-item-update.md)
- [{#T}](./sale-basket-item-get.md)
- [{#T}](./sale-basket-item-delete.md)
- [{#T}](./sale-basket-item-get-fields.md)
- [{#T}](./sale-basket-item-add-catalog-product.md)
- [{#T}](./sale-basket-item-update-catalog-product.md)
- [{#T}](./sale-basket-item-get-catalog-product-fields.md)
