# Get the list of orders sale.order.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `sale.order.list` retrieves a list of orders with filtering, sorting, and pagination. The response contains only order fields: basket items, payments, shipments, and property values are returned by the [sale.order.get](./sale-order-get.md) method.

{% note warning "" %}

The method does not validate field names in `select` and `filter`. A condition with a misspelled field name is not applied, and the method returns orders as if the condition were absent. The exact field names are returned by [sale.order.getFields](./sale-order-get-fields.md).

{% endnote %}

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | Order fields to return. The field names are listed in the [sale_order](../data-types.md#sale_order) object.

If the parameter is not provided, the array is empty, or it contains no existing field, the method returns all order fields ||
|| **filter**
[`object`](../../data-types.md) | Order filter conditions in the format `{"field_1": "value_1", ... "field_N": "value_N"}`, where `field` is a field of the [sale_order](../data-types.md#sale_order) object.

You can add a prefix to the key to set the comparison condition:

- `=` — equal to, used by default
- `!=` or `!` — not equal to
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — in the list, the value is an array
- `!@` — not in the list, the value is an array
- `%` — contains a substring, do not pass the `%` character in the value
- `!%` — does not contain a substring, do not pass the `%` character in the value
- `=%` or `%=` — LIKE by pattern, pass the `%` character in the value: `mol%` — starts with "mol", `%mol` — ends with "mol", `%mol%` — contains "mol"
- `!=%` or `!%=` — NOT LIKE by pattern, pass the `%` character in the value ||
|| **order**
[`object`](../../data-types.md) | Sort order in the format `{"field_1": "order_1", ... "field_N": "order_N"}`, where `field` is a field of the [sale_order](../data-types.md#sale_order) object and `order` is the direction:

- `asc` — ascending
- `desc` — descending

If the parameter is not provided, orders are sorted by `id` in ascending order ||
|| **start**
[`integer`](../../data-types.md) | Offset for pagination. A page contains up to 50 orders, and the page size cannot be changed.

The value is calculated using the formula `start = (N-1) * 50`, where `N` is the page number. For the second page, pass `50`. The default is `0` ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","accountNumber","statusId","price","currency","payed","dateInsert","userId"],"filter":{"<id":1000,"@personTypeId":[3,4],"payed":"N"},"order":{"id":"desc"},"start":0}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.order.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","accountNumber","statusId","price","currency","payed","dateInsert","userId"],"filter":{"<id":1000,"@personTypeId":[3,4],"payed":"N"},"order":{"id":"desc"},"start":0,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.order.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type SaleOrderListResult = {
      orders: {
        accountNumber: string
        currency: string
        dateInsert: ISODate | null
        id: number
        payed: string
        price: number
        statusId: string
        userId: number
      }[]
    }

    try {
      // sale.order.list returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<SaleOrderListResult>({
        method: 'sale.order.list',
        params: {
          select: [
            'id',
            'accountNumber',
            'statusId',
            'price',
            'currency',
            'payed',
            'dateInsert',
            'userId',
          ],
          filter: {
            '<id': 1000,
            '@personTypeId': [3, 4],
            payed: 'N',
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
        console.info(`Fetched ${result.orders.length} orders, first order id: ${result.orders[0]?.id}`)
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
      async function fetchOrderList() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // sale.order.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'sale.order.list',
            params: {
              select: [
                'id',
                'accountNumber',
                'statusId',
                'price',
                'currency',
                'payed',
                'dateInsert',
                'userId',
              ],
              filter: {
                '<id': 1000,
                '@personTypeId': [3, 4],
                payed: 'N',
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
          console.info(`Fetched ${result.orders.length} orders, first order id: ${result.orders[0]?.id}`)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', fetchOrderList)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.sale.order.list(
            select=[
                "id",
                "accountNumber",
                "statusId",
                "price",
                "currency",
                "payed",
                "dateInsert",
                "userId",
            ],
            filter={
                "<id": 1000,
                "@personTypeId": [
                    3,
                    4,
                ],
                "payed": "N",
            },
            order={
                "id": "desc",
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
                'sale.order.list',
                [
                    'select' => [
                        'id',
                        'accountNumber',
                        'statusId',
                        'price',
                        'currency',
                        'payed',
                        'dateInsert',
                        'userId',
                    ],
                    'filter' => [
                        '<id'          => 1000,
                        '@personTypeId' => [3, 4],
                        'payed'        => 'N',
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
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching order list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "sale.order.list", {
            "select": [
                "id",
                "accountNumber",
                "statusId",
                "price",
                "currency",
                "payed",
                "dateInsert",
                "userId",
            ],
            "filter": {
                "<id": 1000,
                "@personTypeId": [3, 4],
                "payed": "N",
            },
            "order": {
                "id": "desc",
            },
            "start": 0
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.info(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sale.order.list',
        [
            'select' => [
                "id",
                "accountNumber",
                "statusId",
                "price",
                "currency",
                "payed",
                "dateInsert",
                "userId",
            ],
            'filter' => [
                "<id" => 1000,
                "@personTypeId" => [3, 4],
                "payed" => "N",
            ],
            'order' => [
                "id" => "desc",
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
    res, err := client.Core().Call(ctx, "sale.order.list", b24.Params{
    	"select": []string{"id", "accountNumber", "statusId", "price", "currency", "payed", "dateInsert", "userId"},
    	"filter": b24.Params{
    		"<id":           1000,
    		"@personTypeId": []int{3, 4},
    		"payed":         "N",
    	},
    	"order": b24.Params{
    		"id": "desc",
    	},
    	"start": 0,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("sale.order.list: %w", err)
    }

    // The response arrives as json.RawMessage — unmarshal it
    // into a struct matching the response shape shown below on this page.
    fmt.Printf("%s\n", res.Result)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "orders": [
            {
                "accountNumber": "923",
                "currency": "USD",
                "dateInsert": "2026-09-23T09:06:07+02:00",
                "id": 923,
                "payed": "N",
                "price": 300,
                "statusId": "N",
                "userId": 1
            },
            {
                "accountNumber": "909",
                "currency": "USD",
                "dateInsert": "2026-09-04T23:03:40+02:00",
                "id": 909,
                "payed": "N",
                "price": 100.5,
                "statusId": "N",
                "userId": 1295
            }
        ]
    },
    "next": 50,
    "total": 189,
    "time": {
        "start": 1790576208,
        "finish": 1790576208.881895,
        "duration": 0.8818950653076172,
        "processing": 0,
        "date_start": "2026-09-28T09:16:48+02:00",
        "date_finish": "2026-09-28T09:16:48+02:00",
        "operating_reset_at": 1790576808,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | The root element of the response [(detailed description)](#result) ||
|| **total**
[`integer`](../../data-types.md) | The total number of orders matching the filter ||
|| **next**
[`integer`](../../data-types.md) | The `start` value for the next page. Returned only if there are more orders after the current page ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Object result {#result}

#|
|| **Name**
`type` | **Description** ||
|| **orders**
[`sale_order[]`](../data-types.md#sale_order) | An array of orders, up to 50 per call. The set of fields is defined by `select` ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "100",
    "error_description": "Invalid order \"SIDEWAYS\""
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `100` | `Invalid order "SIDEWAYS"` | A sort direction other than `asc` and `desc` is passed in `order` ||
|| `400` | `200040300010` | `Access Denied` | Insufficient permissions to read orders ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./sale-order-add.md)
- [{#T}](./sale-order-update.md)
- [{#T}](./sale-order-get.md)
- [{#T}](./sale-order-delete.md)
- [{#T}](./sale-order-get-fields.md)

