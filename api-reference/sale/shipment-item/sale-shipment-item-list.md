# Get a list of shipment item table elements sale.shipmentitem.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `sale.shipmentitem.list` allows you to retrieve a list of shipment item table elements.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | An array containing the list of fields to select (see fields of the object [sale_order_shipment_item](../data-types.md#sale_order_shipment_item)).

If not provided or an empty array is passed, all available fields of the shipment item table elements will be selected. ||
|| **filter**
[`object`](../../data-types.md) | An object for filtering the selected shipment item table elements in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

Possible values for `field` correspond to the fields of the object [sale_order_shipment_item](../data-types.md#sale_order_shipment_item).

An additional prefix can be assigned to the key to specify the filter behavior. Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN (an array is passed as the value)
- `!@`— NOT IN (an array is passed as the value)
- `%` — LIKE, substring search. The `%` symbol in the filter value does not need to be passed. The search looks for the substring in any position of the string.
- `=%` — LIKE, substring search. The `%` symbol needs to be passed in the value. Examples:
    - "mol%" — searching for values starting with "mol"
    - "%mol" — searching for values ending with "mol"
    - "%mol%" — searching for values where "mol" can be in any position

- `%=` — LIKE (see description above)

- `!%` — NOT LIKE, substring search. The `%` symbol in the filter value does not need to be passed. The search goes from both sides.

- `!=%` — NOT LIKE, substring search. The `%` symbol needs to be passed in the value. Examples:
    - "mol%" — searching for values not starting with "mol"
    - "%mol" — searching for values not ending with "mol"
    - "%mol%" — searching for values where the substring "mol" is not present in any position

- `!%=` — NOT LIKE (see description above)

- `=` — equal, exact match (used by default)
- `!=` - not equal
- `!` — not equal
 ||
|| **order**
[`object`](../../data-types.md) | An object for sorting the selected shipment item table elements in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` correspond to the fields of the object [sale_order_shipment_item](../data-types.md#sale_order_shipment_item).

Possible values for `order`:
- `asc` — in ascending order
- `desc` — in descending order ||
|| **start**
[`integer`](../../data-types.md) | This parameter is used to control pagination.

The page size of results is always static: 50 records.

To select the second page of results, you need to pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the value of the `start` parameter:

`start = (N-1) * 50`, where `N` — the desired page number ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```curl
    -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","orderDeliveryId","basketId","quantity","xmlId","dateInsert","reservedQuantity"],"filter":{"<id":10,"@orderDeliveryId":[2431,2430],"basketId":2716},"order":{"id":"desc"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.shipmentitem.list
    ```

- cURL (OAuth)

    ```curl
    -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","orderDeliveryId","basketId","quantity","xmlId","dateInsert","reservedQuantity"],"filter":{"<id":10,"@orderDeliveryId":[2431,2430],"basketId":2716},"order":{"id":"desc"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.shipmentitem.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type ShipmentItemListResult = {
      shipmentItems: {
        id: number
        orderDeliveryId: number
        basketId: number
        quantity: number
        xmlId: string
        dateInsert: ISODate | null
        reservedQuantity: number
      }[]
    }

    // sale.shipmentitem.list returns a single page (max 50 records). For the whole result set
    // use a list helper: $b24.actions.v2.callList.make() returns every record as one
    // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
    // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
    // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
    try {
      const response = await $b24.actions.v2.call.make<ShipmentItemListResult>({
        method: 'sale.shipmentitem.list',
        params: {
          select: [
            'id',
            'orderDeliveryId',
            'basketId',
            'quantity',
            'xmlId',
            'dateInsert',
            'reservedQuantity',
          ],
          filter: {
            '<id': 10,
            '@orderDeliveryId': [2431, 2430],
            basketId: 2716,
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
        console.info('Shipment items count:', result.shipmentItems.length, result.shipmentItems)
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
      async function listShipmentItems() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // sale.shipmentitem.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'sale.shipmentitem.list',
            params: {
              select: [
                'id',
                'orderDeliveryId',
                'basketId',
                'quantity',
                'xmlId',
                'dateInsert',
                'reservedQuantity',
              ],
              filter: {
                '<id': 10,
                '@orderDeliveryId': [2431, 2430],
                basketId: 2716,
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
          console.info('Shipment items count:', result.shipmentItems.length, result.shipmentItems)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listShipmentItems)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'sale.shipmentitem.list',
                [
                    'select' => [
                        'id',
                        'orderDeliveryId',
                        'basketId',
                        'quantity',
                        'xmlId',
                        'dateInsert',
                        'reservedQuantity',
                    ],
                    'filter' => [
                        '<id'            => 10,
                        '@orderDeliveryId' => [2431, 2430],
                        'basketId'       => 2716,
                    ],
                    'order' => [
                        'id' => 'desc',
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching shipment items: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "sale.shipmentitem.list", {
            "select": [
                "id",
                "orderDeliveryId",
                "basketId",
                "quantity",
                "xmlId",
                "dateInsert",
                "reservedQuantity",
            ],
            "filter": {
                "<id": 10,
                "@orderDeliveryId": [2431, 2430],
                "basketId": 2716,
            },
            "order": {
                "id": "desc",
            }
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
        'sale.shipmentitem.list',
        [
            'select' => [
                "id",
                "orderDeliveryId",
                "basketId",
                "quantity",
                "xmlId",
                "dateInsert",
                "reservedQuantity",
            ],
            'filter' => [
                "<id" => 10,
                "@orderDeliveryId" => [2431, 2430],
                "basketId" => 2716,
            ],
            'order' => [
                "id" => "desc",
            ]
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
        "shipmentItems": [
            {
                "basketId": 2716,
                "dateInsert": "2024-04-11T09:10:34+02:00",
                "id": 7,
                "orderDeliveryId": 2431,
                "quantity": 5,
                "reservedQuantity": 0,
                "xmlId": "myNewXmlId"
            },
            {
                "basketId": 2716,
                "dateInsert": "2024-04-08T10:36:24+02:00",
                "id": 1,
                "orderDeliveryId": 2430,
                "quantity": 5,
                "reservedQuantity": 0,
                "xmlId": "myXmlId2"
            }
        ]
    },
    "total": 2,
    "time": {
        "start": 1712819741.592596,
        "finish": 1712819741.796288,
        "duration": 0.20369195938110352,
        "processing": 0.01679706573486328,
        "date_start": "2024-04-11T10:15:41+02:00",
        "date_finish": "2024-04-11T10:15:41+02:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **shipmentItems**
[`sale_order_shipment_item[]`](../data-types.md) | An array of objects with information about the selected shipment item table elements ||
|| **total**
[`integer`](../../data-types.md) | Total number of records found ||
|| **time**
[`time`](../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": 0,
    "error_description": "error"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300010` | Insufficient permissions to read the shipment item table element ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./sale-shipment-item-add.md)
- [{#T}](./sale-shipment-item-update.md)
- [{#T}](./sale-shipment-item-get.md)
- [{#T}](./sale-shipment-item-delete.md)
- [{#T}](./sale-shipment-item-get-fields.md)

