# Get a list of shipment property values sale.shipmentpropertyvalue.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method returns a list of shipment property values.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | An array of fields to select (see fields of the [sale_shipment_property_value](../data-types.md#sale_shipment_property_value) object).

If the array is not provided or an empty array is passed, all available property value fields will be selected.
||
|| **filter**
[`object`](../../data-types.md) | An object for filtering selected property values in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

Possible values for `field` correspond to the fields of the [sale_shipment_property_value](../data-types.md#sale_shipment_property_value) object.

An additional prefix can be assigned to the key to specify the filter behavior. Possible prefix values:
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

Possible values for `field` correspond to the fields of the [sale_shipment_property_value](../data-types.md#sale_shipment_property_value) object.

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

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["code","id","name","shipmentId","shipmentPropsId","shipmentPropsXmlId","value"],"filter":{"@shipmentId":[4120]},"order":{"shipmentId":"desc"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.shipmentpropertyvalue.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["code","id","name","shipmentId","shipmentPropsId","shipmentPropsXmlId","value"],"filter":{"@shipmentId":[4120]},"order":{"shipmentId":"desc"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.shipmentpropertyvalue.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type ShipmentPropertyValueResult = {
      propertyValues: {
        code: string | null
        id: number
        name: string
        shipmentId: number
        shipmentPropsId: number
        shipmentPropsXmlId: string
        value: string
      }[]
    }

    // sale.shipmentpropertyvalue.list returns a single page (max 50 records). For the whole result set
    // use a list helper: $b24.actions.v2.callList.make() returns every record as one
    // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
    // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
    // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
    try {
      const response = await $b24.actions.v2.call.make<ShipmentPropertyValueResult>({
        method: 'sale.shipmentpropertyvalue.list',
        params: {
          select: [
            'code',
            'id',
            'name',
            'shipmentId',
            'shipmentPropsId',
            'shipmentPropsXmlId',
            'value',
          ],
          filter: {
            '@shipmentId': [4120],
          },
          order: {
            shipmentId: 'desc',
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
        console.info('Property values:', result.propertyValues, 'Count on page:', result.propertyValues.length)
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
      async function listShipmentPropertyValues() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // sale.shipmentpropertyvalue.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'sale.shipmentpropertyvalue.list',
            params: {
              select: [
                'code',
                'id',
                'name',
                'shipmentId',
                'shipmentPropsId',
                'shipmentPropsXmlId',
                'value',
              ],
              filter: {
                '@shipmentId': [4120],
              },
              order: {
                shipmentId: 'desc',
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
          console.info('Property values:', result.propertyValues, 'Count on page:', result.propertyValues.length)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listShipmentPropertyValues)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'sale.shipmentpropertyvalue.list',
                [
                    'select' => [
                        'code',
                        'id',
                        'name',
                        'shipmentId',
                        'shipmentPropsId',
                        'shipmentPropsXmlId',
                        'value',
                    ],
                    'filter' => [
                        '@shipmentId' => [4120],
                    ],
                    'order' => [
                        'shipmentId' => 'desc',
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching shipment property values: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "sale.shipmentpropertyvalue.list", {
            "select": [
                "code",
                "id",
                "name",
                "shipmentId",
                "shipmentPropsId",
                "shipmentPropsXmlId",
                "value",
            ],
            "filter": {
                "@shipmentId": [4120],
            },
            "order": {
                "shipmentId": "desc",
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
        'sale.shipmentpropertyvalue.list',
        [
            'select' => [
                "code",
                "id",
                "name",
                "shipmentId",
                "shipmentPropsId",
                "shipmentPropsXmlId",
                "value",
            ],
            'filter' => [
                "@shipmentId" => [4120],
            ],
            'order' => [
                "shipmentId" => "desc",
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Successful Response

HTTP status: **200**

```json
{
    "result": {
        "propertyValues": [
            {
                "code": null,
                "id": 38164,
                "name": "Comments",
                "shipmentId": 4120,
                "shipmentPropsId": 105,
                "shipmentPropsXmlId": "bx_6661d1b6690d5",
                "value": "Comments value"
            },
            {
                "code": null,
                "id": 38166,
                "name": "Description",
                "shipmentId": 4120,
                "shipmentPropsId": 106,
                "shipmentPropsXmlId": "bx_6666cc212db52",
                "value": "Description value"
            }
        ]
    },
    "total": 2,
    "time": {
        "start": 1718023945.124614,
        "finish": 1718023945.312019,
        "duration": 0.18740510940551758,
        "processing": 0.016345977783203125,
        "date_start": "2024-06-10T15:52:25+02:00",
        "date_finish": "2024-06-10T15:52:25+02:00"
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
[`sale_shipment_property_value[]`](../data-types.md#sale_shipment_property_value) | An array of objects with information about the selected shipment property values ||
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
|| `200040300010` | Insufficient permissions to read property values ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include notitle [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sale-shipment-property-value-modify.md)
- [{#T}](./sale-shipment-property-value-get.md)
- [{#T}](./sale-shipment-propertyvalue-delete.md)
- [{#T}](./sale-shipment-property-value-get-fields.md)

