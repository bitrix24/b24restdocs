# Get a list of property bindings sale.propertyRelation.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `sale.propertyRelation.list` allows you to retrieve a list of property bindings.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | The array contains a list of fields to select (see fields of the object [sale_order_property_relation](../data-types.md#sale_order_property_relation)).

If not provided or an empty array is passed, all available fields of property bindings will be selected. ||
|| **filter**
[`object`](../../data-types.md) | An object for filtering the selected property bindings in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

Possible values for `field` correspond to the fields of the object [sale_order_property_relation](../data-types.md#sale_order_property_relation).

An additional prefix can be assigned to the key to clarify the filter behavior. Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN (an array is passed as the value)
- `!@`— NOT IN (an array is passed as the value)
- `%` — LIKE, substring search. The `%` symbol in the filter value should not be passed. The search looks for the substring in any position of the string
- `=%` — LIKE, substring search. The `%` symbol should be passed in the value. Examples:
    - "mol%" — searching for values starting with "mol"
    - "%mol" — searching for values ending with "mol"
    - "%mol%" — searching for values where "mol" can be in any position

- `%=` — LIKE (see description above)

- `!%` — NOT LIKE, substring search. The `%` symbol in the filter value should not be passed. The search goes from both sides.

- `!=%` — NOT LIKE, substring search. The `%` symbol should be passed in the value. Examples:
    - "mol%" — searching for values not starting with "mol"
    - "%mol" — searching for values not ending with "mol"
    - "%mol%" — searching for values where the substring "mol" is not present in any position

- `!%=` — NOT LIKE (see description above)

- `=` — equal, exact match (used by default)
- `!=` - not equal
- `!` — not equal
 ||
|| **order**
[`object`](../../data-types.md) | An object for sorting the selected property bindings in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` correspond to the fields of the object [sale_order_property_relation](../data-types.md#sale_order_property_relation).

Possible values for `order`:
- `asc` — in ascending order
- `desc` — in descending order ||
|| **start**
[`integer`](../../data-types.md) | The parameter is used to control pagination.

The page size of results is always static: 50 records.

To select the second page of results, you need to pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` — the number of the desired page ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```curl
    -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["entityId","entityType","propertyId"],"filter":{"entityId":6},"order":{"entityId":"asc"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.propertyRelation.list
    ```

- cURL (OAuth)

    ```curl
    -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["entityId","entityType","propertyId"],"filter":{"entityId":6},"order":{"entityId":"asc"},    "auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.propertyRelation.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type PropertyRelationListResult = {
      propertyRelations: {
        entityId: number
        entityType: string
        propertyId: number
      }[]
    }

    try {
      // sale.propertyRelation.list returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<PropertyRelationListResult>({
        method: 'sale.propertyRelation.list',
        params: {
          select: [
            'entityId',
            'entityType',
            'propertyId',
          ],
          filter: {
            entityId: 6,
          },
          order: {
            entityId: 'asc',
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
        console.info(result.propertyRelations.length, result.propertyRelations)
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
      async function listPropertyRelations() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // sale.propertyRelation.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'sale.propertyRelation.list',
            params: {
              select: [
                'entityId',
                'entityType',
                'propertyId',
              ],
              filter: {
                entityId: 6,
              },
              order: {
                entityId: 'asc',
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
          console.info(result.propertyRelations.length, result.propertyRelations)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listPropertyRelations)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'sale.propertyRelation.list',
                [
                    'select' => [
                        'entityId',
                        'entityType',
                        'propertyId',
                    ],
                    'filter' => [
                        'entityId' => 6,
                    ],
                    'order' => [
                        'entityId' => 'asc',
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error listing sale property relations: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'sale.propertyRelation.list', 
        {
            select:[
                'entityId',
                'entityType',
                'propertyId',
            ],
            filter:{
                entityId: 6,
            },
            order:{
                entityId: 'asc',
            },
        }, 
        function(result)
        {
            if(result.error())
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
        'sale.propertyRelation.list',
        [
            'select' => [
                'entityId',
                'entityType',
                'propertyId',
            ],
            'filter' => [
                'entityId' => 6,
            ],
            'order' => [
                'entityId' => 'asc',
            ],
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
        "propertyRelations": [
            {
                "entityId": 6,
                "entityType": "L",
                "propertyId": 39
            },
            {
                "entityId": 6,
                "entityType": "D",
                "propertyId": 40
            },
            {
                "entityId": 6,
                "entityType": "L",
                "propertyId": 40
            },
        ]
    },
    "total": 3,
    "time": {
        "start": 1712308456.66363,
        "finish": 1712308457.096419,
        "duration": 0.4327890872955322,
        "processing": 0.023340940475463867,
        "date_start": "2024-04-05T12:14:16+02:00",
        "date_finish": "2024-04-05T12:14:17+02:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | The root element of the response ||
|| **propertyRelations**
[`sale_order_property_relation`](../data-types.md) | An array of objects with information about the selected property bindings ||
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
|| `200040300010` | Insufficient permissions to read available fields of property bindings ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./sale-property-relation-add.md)
- [{#T}](./sale-property-relation-delete-by-filter.md)
- [{#T}](./sale-property-relation-get-fields.md)

