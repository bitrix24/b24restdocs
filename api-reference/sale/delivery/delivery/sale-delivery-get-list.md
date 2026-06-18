# Get a list of delivery services sale.delivery.getlist

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`sale`](../../../scopes/permissions.md)
>
> Who can execute the method: CRM administrator

This method retrieves a list of delivery services.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **SELECT**
[`array`](../../data-types.md) | An array containing the list of fields to select (see fields of the object [`sale_delivery_service`](../../data-types.md)).
 
If not provided or an empty array is passed, all available fields of delivery services will be selected.
||
|| **FILTER**
[`object`](../../data-types.md) | An object for filtering the selected delivery services in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.
 
Possible values for `field` correspond to the fields of the object [`sale_delivery_service`](../../data-types.md).

An additional prefix can be assigned to the key to specify the filter behavior. Possible prefix values:

- `=` — equals (works with arrays as well)
- `%` — LIKE, substring search. The % symbol in the filter value does not need to be passed. The search looks for the substring at any position in the string.
- `>` — greater than
- `<` — less than
- `!=` — not equal
- `!%` — NOT LIKE, substring search. The % symbol in the filter value does not need to be passed. The search goes from both sides.
- `>=` — greater than or equal to
- `<=` — less than or equal to
- `=%` — LIKE, substring search. The % symbol needs to be passed in the value. Examples: 
    - `"mol%"` — searching for values starting with "mol"
    - `"%mol"` — searching for values ending with "mol"
    - `"%mol%"` — searching for values where "mol" can be at any position
- `%=` — LIKE (see description above)
- `!=%` — NOT LIKE, substring search. The % symbol needs to be passed in the value. Examples:
    - `"mol%"` — searching for values not starting with "mol"
    - `"%mol"` — searching for values not ending with "mol"
    - `"%mol%"` — searching for values where the substring "mol" is not present at any position
- `!%=` — NOT LIKE (see description above)
||
|| **ORDER**
[`object`](../../data-types.md) | An object for sorting the selected delivery services in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.
 
Possible values for `field` correspond to the fields of the object [`sale_delivery_service`](../../data-types.md).
 
Possible values for `order`:

- `asc` — in ascending order
- `desc` — in descending order
 ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"SELECT":["ID","PARENT_ID","NAME","ACTIVE","DESCRIPTION","SORT","CURRENCY"],"FILTER":{"@ID":[196,197,198]},"ORDER":{"SORT":"ASC","ID":"DESC"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.delivery.getlist
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"SELECT":["ID","PARENT_ID","NAME","ACTIVE","DESCRIPTION","SORT","CURRENCY"],"FILTER":{"@ID":[196,197,198]},"ORDER":{"SORT":"ASC","ID":"DESC"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.delivery.getlist
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each DeliveryService returned in result[]
    type DeliveryService = {
      ID: number
      PARENT_ID: number | null
      NAME: string
      ACTIVE: string
      DESCRIPTION: string
      SORT: number
      CURRENCY: string
    }

    try {
      // sale.delivery.getlist returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<DeliveryService[]>({
        method: 'sale.delivery.getlist',
        params: {
          SELECT: ['ID', 'PARENT_ID', 'NAME', 'ACTIVE', 'DESCRIPTION', 'SORT', 'CURRENCY'],
          FILTER: {
            '@ID': [196, 197, 198],
          },
          ORDER: {
            SORT: 'ASC',
            ID: 'DESC',
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
        console.info('Delivery services (%d):', result.length, result)
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
      async function getDeliveryList() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // sale.delivery.getlist returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'sale.delivery.getlist',
            params: {
              SELECT: ['ID', 'PARENT_ID', 'NAME', 'ACTIVE', 'DESCRIPTION', 'SORT', 'CURRENCY'],
              FILTER: {
                '@ID': [196, 197, 198],
              },
              ORDER: {
                SORT: 'ASC',
                ID: 'DESC',
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
          console.info('Delivery services (%d):', result.length, result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getDeliveryList)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'sale.delivery.getlist',
                [
                    'SELECT' => [
                        "ID",
                        "PARENT_ID",
                        "NAME",
                        "ACTIVE",
                        "DESCRIPTION",
                        "SORT",
                        "CURRENCY",
                    ],
                    'FILTER' => [
                        '@ID' => [196, 197, 198],
                    ],
                    'ORDER' => [
                        'SORT' => "ASC",
                        'ID'   => "DESC",
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting delivery list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'sale.delivery.getlist', {
            SELECT: [
                "ID",
                "PARENT_ID",
                "NAME",
                "ACTIVE",
                "DESCRIPTION",
                "SORT",
                "CURRENCY",
            ],
            FILTER: {
                "@ID": [196, 197, 198],
            },
            ORDER: {
                SORT: "ASC",
                ID: "DESC",
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
        'sale.delivery.getlist',
        [
            'SELECT' => [
                "ID",
                "PARENT_ID",
                "NAME",
                "ACTIVE",
                "DESCRIPTION",
                "SORT",
                "CURRENCY",
            ],
            'FILTER' => [
                "@ID" => [196, 197, 198],
            ],
            'ORDER' => [
                "SORT" => "ASC",
                "ID" => "DESC",
            ]
        ]
    );
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result":[
        {
            "NAME":"Cargo",
            "ACTIVE":"Y",
            "DESCRIPTION":"Cargo Delivery",
            "CURRENCY":"USD",
            "ID":198,
            "PARENT_ID":196,
            "SORT":500
        },
        {
            "NAME":"Taxi",
            "ACTIVE":"Y",
            "DESCRIPTION":"Taxi Delivery",
            "CURRENCY":"USD",
            "ID":197,
            "PARENT_ID":196,
            "SORT":500
        },
        {
            "NAME":"Uber Taxi",
            "ACTIVE":"Y",
            "DESCRIPTION":"Uber Taxi Description",
            "CURRENCY":"USD",
            "ID":196,
            "PARENT_ID":null,
            "SORT":500
        }
    ],
    "time":{
        "start":1714141661.751638,
        "finish":1714141661.945714,
        "duration":0.1940760612487793,
        "processing":0.013057947158813477,
        "date_start":"2024-04-26T17:27:41+02:00",
        "date_finish":"2024-04-26T17:27:41+02:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`sale_delivery_service[]`](../../data-types.md) | An array of objects containing information about the selected delivery services ||
|| **time**
[`time`](../../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**, **403**

```json
{
   "error":"ACCESS_DENIED",
   "error_description":"Access denied!"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Status** ||
|| `ERROR_CHECK_FAILURE` | Validation error of incoming parameters (details in the error description) | 400 ||
|| `ACCESS_DENIED` | Insufficient rights to retrieve the list of delivery services | 403 ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sale-delivery-add.md)
- [{#T}](./sale-delivery-delete.md)
- [{#T}](./sale-delivery-update.md)
- [{#T}](./sale-delivery-config-update.md)
- [{#T}](./sale-delivery-config-get.md)

