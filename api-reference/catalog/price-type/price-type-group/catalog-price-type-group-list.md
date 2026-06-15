# Get a List of Price Type Bindings to Customer Groups catalog.priceTypeGroup.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View Product Catalog" or "Manage Price Types" access permission.

The method `catalog.priceTypeGroup.list` returns a list of price type bindings to customer groups.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md)| An array of fields from [catalog_price_type_group](../../data-types.md#catalog_price_type_group) that need to be selected.

If the array is not provided or an empty array is passed, all available binding fields will be selected.
||
|| **filter**
[`object`](../../data-types.md)| An object for filtering the selected bindings in the format `{"field_1": "value_1", ..., "field_N": "value_N"}`.

Possible values for `field` correspond to the fields of the [catalog_price_type_group](../../data-types.md#catalog_price_type_group) object.

An additional prefix can be assigned to the key to specify the filter behavior. Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `%` — LIKE, substring search. The `%` symbol should not be included in the filter value
- `=%` — LIKE, substring search. The `%` symbol should be included in the value
- `%=` — LIKE (similar to `=%`)
- `!%` — NOT LIKE, substring search. The `%` symbol should not be included in the filter value
- `!=%` — NOT LIKE, substring search. The `%` symbol should be included in the value
- `!%=` — NOT LIKE (similar to `!=%`)
- `=` — equal, exact match (used by default)
- `!=` — not equal
- `!` — not equal

If the filter conditions do not match any records, the method will return an empty list.
||
|| **order**
[`object`](../../data-types.md)| An object for sorting the selected bindings in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` correspond to the fields of the [catalog_price_type_group](../../data-types.md#catalog_price_type_group) object.

Possible values for `order`:

- `asc` — in ascending order
- `desc` — in descending order
||
|| **start**
[`integer`](../../data-types.md)| This parameter is used for managing pagination.

The page size of results is always static — 50 records.

To select the second page of results, pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` is the desired page number
||
|#

## Code Examples

{% include [Example Notes](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","catalogGroupId","groupId","access"],"filter":{"catalogGroupId":9,"groupId":23},"order":{"id":"ASC"},"start":0}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.priceTypeGroup.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","catalogGroupId","groupId","access"],"filter":{"catalogGroupId":9,"groupId":23},"order":{"id":"ASC"},"start":0,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.priceTypeGroup.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type PriceTypeGroupsResult = {
      priceTypeGroups: {
        id: number
        catalogGroupId: number
        groupId: number
        access: string
      }[]
    }

    try {
      // catalog.priceTypeGroup.list returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<PriceTypeGroupsResult>({
        method: 'catalog.priceTypeGroup.list',
        params: {
          select: ['id', 'catalogGroupId', 'groupId', 'access'],
          filter: { catalogGroupId: 9, groupId: 23 },
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
        console.info('Price type groups:', result.priceTypeGroups, 'Count:', result.priceTypeGroups.length)
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
      async function fetchPriceTypeGroupList() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // catalog.priceTypeGroup.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'catalog.priceTypeGroup.list',
            params: {
              select: ['id', 'catalogGroupId', 'groupId', 'access'],
              filter: { catalogGroupId: 9, groupId: 23 },
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
          console.info('Price type groups:', result.priceTypeGroups, 'Count:', result.priceTypeGroups.length)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', fetchPriceTypeGroupList)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'catalog.priceTypeGroup.list',
                [
                    'select' => ['id', 'catalogGroupId', 'groupId', 'access'],
                    'filter' => [
                        'catalogGroupId' => 9,
                        'groupId' => 23
                    ],
                    'order' => [
                        'id' => 'ASC'
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        $result->next();
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error listing price type groups: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.priceTypeGroup.list',
        {
            select: ['id', 'catalogGroupId', 'groupId', 'access'],
            filter: {
                catalogGroupId: 9,
                groupId: 23
            },
            order: {
                id: 'ASC'
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
        'catalog.priceTypeGroup.list',
        [
            'select' => ['id', 'catalogGroupId', 'groupId', 'access'],
            'filter' => [
                'catalogGroupId' => 9,
                'groupId' => 23
            ],
            'order' => [
                'id' => 'ASC'
            ],
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
        "priceTypeGroups": [
            {
                "access": "Y",
                "catalogGroupId": 9,
                "groupId": 23,
                "id": 109
            },
            {
                "access": "N",
                "catalogGroupId": 9,
                "groupId": 23,
                "id": 111
            }
        ]
    },
    "total": 2,
    "time": {
        "start": 1774259997,
        "finish": 1774259997.152525,
        "duration": 0.1525249481201172,
        "processing": 0,
        "date_start": "2026-03-23T12:59:57+01:00",
        "date_finish": "2026-03-23T12:59:57+01:00",
        "operating_reset_at": 1774260597,
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
|| **priceTypeGroups**
[`catalog_price_type_group[]`](../../data-types.md#catalog_price_type_group) | An array of objects containing information about the selected price type bindings to customer groups, structure depends on the `select` parameter ||
|| **total**
[`integer`](../../data-types.md) | The total number of records found ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": 200040300010,
    "error_description": "Access Denied"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `200040300010` | Access Denied | Insufficient permissions to view the catalog ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-price-type-group-add.md)
- [{#T}](./catalog-price-type-group-delete.md)
- [{#T}](./catalog-price-type-group-get-fields.md)