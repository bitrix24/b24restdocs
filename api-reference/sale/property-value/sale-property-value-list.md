# Get a list of property values sale.propertyvalue.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method retrieves a list of order property value options.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | An array of fields to be selected (see fields of the [sale_order_property_value](../data-types.md) object).

If the array is not provided or an empty array is passed, all available property value fields will be selected.
||
|| **filter**
[`object`](../../data-types.md) | An object for filtering selected property values in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

Possible values for `field` correspond to the fields of the [sale_order_property_value](../data-types.md) object.

An additional prefix can be assigned to the key to clarify the filter behavior. Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `%` — LIKE, substring search. The `%` symbol in the filter value does not need to be passed. The search looks for a substring at any position in the string.
- `=%` — LIKE, substring search. The `%` symbol needs to be passed in the value. Examples:
    - `"mol%"` — searches for values starting with "mol"
    - `"%mol"` — searches for values ending with "mol"
    - `"%mol%"` — searches for values where "mol" can be at any position
- `%=` — LIKE (similar to `=%`)
- `!%` — NOT LIKE, substring search. The `%` symbol in the filter value does not need to be passed. The search goes from both sides.
- `!=%` — NOT LIKE, substring search. The `%` symbol needs to be passed in the value. Examples:
    - `"mol%"` — searches for values not starting with "mol"
    - `"%mol"` — searches for values not ending with "mol"
    - `"%mol%"` — searches for values where the substring "mol" is not present at any position
- `!%=` — NOT LIKE (similar to `!=%`)
- `=` — equal, exact match (used by default)
- `!=` — not equal
- `!` — not equal
||
|| **order**
[`object`](../../data-types.md) | An object for sorting selected property values in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` correspond to the fields of the [sale_order_property_value](../data-types.md) object.

Possible values for `order`:
- `asc` — in ascending order
- `desc` — in descending order
||
|| **start**
[`integer`](../../data-types.md) | This parameter is used to manage pagination.

The page size of results is always static: 50 records.

To select the second page of results, pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` is the desired page number
||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["code","id","name","orderId","orderPropsId","orderPropsXmlId","value"],"filter":{"=code":"FIO","%value":"Boris",">orderId":1600},"order":{"orderId":"desc"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.propertyvalue.list
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["code","id","name","orderId","orderPropsId","orderPropsXmlId","value"],"filter":{"=code":"FIO","%value":"Boris",">orderId":1600},"order":{"orderId":"desc"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.propertyvalue.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type PropertyValueListResult = {
      propertyValues: Array<{
        code: string
        id: number
        name: string
        orderId: number
        orderPropsId: number
        orderPropsXmlId: string | null
        value: string
      }>
    }

    try {
      // sale.propertyvalue.list returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<PropertyValueListResult>({
        method: 'sale.propertyvalue.list',
        params: {
          select: [
            'code',
            'id',
            'name',
            'orderId',
            'orderPropsId',
            'orderPropsXmlId',
            'value',
          ],
          filter: {
            '=code': 'FIO',
            '%value': 'Boris',
            '>orderId': 1600,
          },
          order: {
            orderId: 'desc',
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
        console.info('Property values count:', result.propertyValues.length)
        console.info('First property value:', result.propertyValues[0])
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
      async function getPropertyValueList() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // sale.propertyvalue.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'sale.propertyvalue.list',
            params: {
              select: [
                'code',
                'id',
                'name',
                'orderId',
                'orderPropsId',
                'orderPropsXmlId',
                'value',
              ],
              filter: {
                '=code': 'FIO',
                '%value': 'Boris',
                '>orderId': 1600,
              },
              order: {
                orderId: 'desc',
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
          console.info('Property values count:', result.propertyValues.length)
          console.info('First property value:', result.propertyValues[0])
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getPropertyValueList)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'sale.propertyvalue.list',
                [
                    'select' => [
                        'code',
                        'id',
                        'name',
                        'orderId',
                        'orderPropsId',
                        'orderPropsXmlId',
                        'value',
                    ],
                    'filter' => [
                        '=code'    => 'FIO',
                        '%value'   => 'Boris',
                        '>orderId' => 1600,
                    ],
                    'order' => [
                        'orderId' => 'desc',
                    ],
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
        echo 'Error fetching sale property values: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "sale.propertyvalue.list", {
            "select": [
                "code",
                "id",
                "name",
                "orderId",
                "orderPropsId",
                "orderPropsXmlId",
                "value",
            ],
            "filter": {
                "=code": "FIO",
                "%value": "Boris",
                ">orderId": 1600,
            },
            "order": {
                "orderId": "desc",
            },
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
        'sale.propertyvalue.list',
        [
            'select' => [
                'code',
                'id',
                'name',
                'orderId',
                'orderPropsId',
                'orderPropsXmlId',
                'value',
            ],
            'filter' => [
                '=code' => 'FIO',
                '%value' => 'Boris',
                '>orderId' => 1600,
            ],
            'order' => [
                'orderId' => 'desc',
            ],
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
        "propertyValues": [
            {
                "code": "FIO",
                "id": 10774,
                "name": "First Last Name",
                "orderId": 1650,
                "orderPropsId": 20,
                "orderPropsXmlId": null,
                "value": "Sokolov Boris Viktorovich"
            },
            {
                "code": "FIO",
                "id": 10763,
                "name": "First Last Name",
                "orderId": 1649,
                "orderPropsId": 20,
                "orderPropsXmlId": null,
                "value": "Sokolov Boris Viktorovich"
            },
            {
                "code": "FIO",
                "id": 10723,
                "name": "First Last Name",
                "orderId": 1641,
                "orderPropsId": 20,
                "orderPropsXmlId": null,
                "value": "Sokolov Boris Viktorovich"
            },
            {
                "code": "FIO",
                "id": 10718,
                "name": "First Last Name",
                "orderId": 1640,
                "orderPropsId": 20,
                "orderPropsXmlId": null,
                "value": "Sokolov Boris Viktorovich"
            },
            {
                "code": "FIO",
                "id": 10713,
                "name": "First Last Name",
                "orderId": 1639,
                "orderPropsId": 20,
                "orderPropsXmlId": null,
                "value": "Sokolov Boris Viktorovich"
            },
            {
                "code": "FIO",
                "id": 10708,
                "name": "First Last Name",
                "orderId": 1638,
                "orderPropsId": 20,
                "orderPropsXmlId": null,
                "value": "Sokolov Boris Viktorovich"
            },
            {
                "code": "FIO",
                "id": 10687,
                "name": "First Last Name",
                "orderId": 1634,
                "orderPropsId": 20,
                "orderPropsXmlId": null,
                "value": "Sokolov Boris Viktorovich"
            },
            {
                "code": "FIO",
                "id": 10517,
                "name": "First Last Name",
                "orderId": 1603,
                "orderPropsId": 20,
                "orderPropsXmlId": null,
                "value": "Sokolov Boris Viktorovich"
            }
        ]
    },
    "total": 8,
    "time": {
        "start": 1712061753.171393,
        "finish": 1712061753.431631,
        "duration": 0.2602381706237793,
        "processing": 0.021820783615112305,
        "date_start": "2024-04-02T15:42:33+02:00",
        "date_finish": "2024-04-02T15:42:33+02:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **propertyValues**
[`sale_order_property_value[]`](../data-types.md) | An array of objects with information about the selected property values ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

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
|| `200040300010` | Insufficient permissions to read property values ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./sale-property-value-modify.md)
- [{#T}](./sale-property-value-get.md)
- [{#T}](./sale-property-value-delete.md)
- [{#T}](./sale-property-value-get-fields.md)

