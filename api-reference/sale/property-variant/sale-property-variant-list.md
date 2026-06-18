# Get a list of property variants sale.propertyvariant.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method retrieves a list of property value variants. This method is applicable only for properties of type `ENUM`.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | An array of fields to be selected (see fields of the object [sale_order_property_variant](../data-types.md)).

If the array is not provided or an empty array is passed, all available fields of the property value variants will be selected. ||
|| **filter**
[`object`](../../data-types.md) | An object for filtering the selected property value variants in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

Possible values for `field` correspond to the fields of the object [sale_order_property_variant](../data-types.md).

An additional prefix can be assigned to the key to specify the filter behavior. Possible prefix values:
- `+` — filtering by the exact value of the specified field; this also includes elements where the field value is undefined (NULL)
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `%` — LIKE, substring search. The % symbol in the filter value does not need to be passed. The search looks for a substring at any position in the string
- `!=` — not equal
- `!` — not equal
||
|| **order**
[`object`](../../data-types.md) | An object for sorting the selected property value variants in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` correspond to the fields of the object [sale_order_property_variant](../data-types.md).

Possible values for `order`:
- `asc` — in ascending order
- `desc` — in descending order
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
    -d '{"select":["id","name","orderPropsId","value"],"filter":{">=id":5},"order":{"orderPropsId":"desc","id":"asc"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.propertyvariant.list
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","name","orderPropsId","value"],"filter":{">=id":5},"order":{"orderPropsId":"desc","id":"asc"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.propertyvariant.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type PropertyVariantResult = {
      propertyVariants: {
        id: number
        name: string
        orderPropsId: number
        value: string
      }[]
    }

    try {
      // sale.propertyvariant.list returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<PropertyVariantResult>({
        method: 'sale.propertyvariant.list',
        params: {
          select: ['id', 'name', 'orderPropsId', 'value'],
          filter: {
            '>=id': 5,
          },
          order: {
            orderPropsId: 'desc',
            id: 'asc',
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
        console.info('Property variants:', result.propertyVariants.length, result.propertyVariants)
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
      async function listPropertyVariants() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // sale.propertyvariant.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'sale.propertyvariant.list',
            params: {
              select: ['id', 'name', 'orderPropsId', 'value'],
              filter: {
                '>=id': 5,
              },
              order: {
                orderPropsId: 'desc',
                id: 'asc',
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
          console.info('Property variants:', result.propertyVariants.length, result.propertyVariants)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listPropertyVariants)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'sale.propertyvariant.list',
                [
                    'select' => ['id', 'name', 'orderPropsId', 'value'],
                    'filter' => [
                        '>=id' => 5,
                    ],
                    'order' => [
                        'orderPropsId' => 'desc',
                        'id' => 'asc',
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching property variants: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "sale.propertyvariant.list", {
            "select": ["id", "name", "orderPropsId", "value"],
            "filter": {
                ">=id": 5,
            },
            "order": {
                "orderPropsId": "desc",
                "id": "asc",
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
        'sale.propertyvariant.list',
        [
            'select' => ['id', 'name', 'orderPropsId', 'value'],
            'filter' => ['>=id' => 5],
            'order' => [
                'orderPropsId' => 'desc',
                'id' => 'asc',
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
        "propertyVariants": [
            {
                "id": 8,
                "name": "M",
                "orderPropsId": 50,
                "value": "m"
            },
            {
                "id": 9,
                "name": "L",
                "orderPropsId": 50,
                "value": "l"
            },
            {
                "id": 6,
                "name": "Red",
                "orderPropsId": 49,
                "value": "red"
            },
            {
                "id": 7,
                "name": "Green",
                "orderPropsId": 49,
                "value": "green"
            }
        ]
    },
    "total": 4,
    "time": {
        "start": 1711633054.631642,
        "finish": 1711633054.872368,
        "duration": 0.24072599411010742,
        "processing": 0.011013984680175781,
        "date_start": "2024-03-28T16:37:34+02:00",
        "date_finish": "2024-03-28T16:37:34+02:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **propertyVariants**
[`sale_order_property_variant[]`](../data-types.md) | Array of objects containing information about the selected property value variants ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
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
|| `200040300010` | Insufficient permissions to read property value variants ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./sale-property-variant-add.md)
- [{#T}](./sale-property-variant-update.md)
- [{#T}](./sale-property-variant-get.md)
- [{#T}](./sale-property-variant-delete.md)
- [{#T}](./sale-property-variant-get-fields.md)

