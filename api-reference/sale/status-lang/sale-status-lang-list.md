# Get a list of localizations for sale.statusLang.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method retrieves a list of localizations for order or delivery statuses.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | An array containing the list of fields to select (see fields of the [sale_status_lang](../data-types.md#sale_status_lang) object).

If not provided or an empty array is passed, all available fields of status localizations will be selected. ||
|| **filter**
[`object`](../../data-types.md) | An object for filtering the selected items in the shipment table part in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

Possible values for `field` correspond to the fields of the [sale_status_lang](../data-types.md#sale_status_lang) object.

An additional prefix can be assigned to the key to clarify the filter's behavior. Possible prefix values:
- `=` — equals (works with arrays as well)
- `%` — LIKE, substring search. The % symbol in the filter value does not need to be passed. The search looks for the substring in any position of the string.
- `>` — greater than
- `<` — less than
- `!=` — not equal
- `!%` — NOT LIKE, substring search. The % symbol in the filter value does not need to be passed. The search goes from both sides.
- `>=` — greater than or equal to
- `<=` — less than or equal to
- `=%` — LIKE, substring search. The % symbol needs to be passed in the value. Examples: 
    - `"mol%"` — searching for values starting with "mol"
    - `"%mol"` — searching for values ending with "mol"
    - `"%mol%"` — searching for values where "mol" can be in any position
- `%=` — LIKE (see description above)
- `!=%` — NOT LIKE, substring search. The % symbol needs to be passed in the value. Examples:
    - `"mol%"` — searching for values not starting with "mol"
    - `"%mol"` — searching for values not ending with "mol"
    - `"%mol%"` — searching for values where the substring "mol" is not present in any position
- `!%=` — NOT LIKE (see description above)
||
|| **order**
[`object`](../../data-types.md) | An object for sorting the selected statuses in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` correspond to the fields of the [sale_status_lang](../data-types.md#sale_status_lang) object.

Possible values for order:

- `asc` — in ascending order
- `desc` — in descending order

||
|| **start**
[`integer`](../../data-types.md) | This parameter is used for managing pagination.
 
The page size of results is always static: 50 records.
 
To select the second page of results, you need to pass the value `50`. To select the third page of results, the value is `100`, and so on.
 
The formula for calculating the `start` parameter value:
 
`start = (N-1) * 50`, where `N` — the desired page number ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["statusId","lid","name","description"],"filter":{"statusId":"N","lid":"de"},"order":{"statusId":"asc"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.statuslang.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["statusId","lid","name","description"],"filter":{"statusId":"N","lid":"de"},"order":{"statusId":"asc"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.statuslang.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type StatusLangListResult = {
      statusLangs: {
        statusId: string
        lid: string
        name: string
        description: string
      }[]
    }

    try {
      // sale.statuslang.list returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<StatusLangListResult>({
        method: 'sale.statuslang.list',
        params: {
          select: ['statusId', 'lid', 'name', 'description'],
          filter: {
            statusId: 'N',
            lid: 'ru',
          },
          order: {
            statusId: 'asc',
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
        console.info('Status langs count:', result.statusLangs.length, result.statusLangs)
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
      async function getStatusLangList() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // sale.statuslang.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'sale.statuslang.list',
            params: {
              select: ['statusId', 'lid', 'name', 'description'],
              filter: {
                statusId: 'N',
                lid: 'ru',
              },
              order: {
                statusId: 'asc',
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
          console.info('Status langs count:', result.statusLangs.length, result.statusLangs)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getStatusLangList)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'sale.statuslang.list',
                [
                    'select' => [
                        'statusId',
                        'lid',
                        'name',
                        'description',
                    ],
                    'filter' => [
                        'statusId' => 'N',
                        'lid'      => 'de',
                    ],
                    'order'  => [
                        'statusId' => 'asc',
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching sale status languages: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "sale.statuslang.list", {
            "select": [
                "statusId",
                "lid",
                "name",
                "description",
            ],
            "filter": {
                "statusId": "N",
                "lid": "de",
            },
            "order": {
                "statusId": "asc",
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
        'sale.statuslang.list',
        [
            'select' => [
                'statusId',
                'lid',
                'name',
                'description',
            ],
            'filter' => [
                'statusId' => 'N',
                'lid' => 'de',
            ],
            'order' => [
                'statusId' => 'asc',
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
        "statusLangs": [
            {
                "description": "Order accepted, but not yet processed (for example, the order has just been created or payment is awaited)",
                "lid": "de",
                "name": "Accepted, awaiting payment",
                "statusId": "N"
            }
        ]
    },
    "total": 1,
    "time": {
        "start": 1712656146.769524,
        "finish": 1712656146.98067,
        "duration": 0.21114587783813477,
        "processing": 0.02018594741821289,
        "date_start": "2024-04-09T12:49:06+02:00",
        "date_finish": "2024-04-09T12:49:06+02:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | The root element of the response ||
|| **statusLangs**
[`sale_status_lang[]`](../data-types.md) | An array of objects with information about the selected status localizations ||
|| **total**
[`integer`](../../data-types.md) | The total number of records found ||
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
|| `200040300010` | Insufficient rights to read status localizations ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./sale-status-lang-get-list-langs.md)
- [{#T}](./sale-status-lang-add.md)
- [{#T}](./sale-status-lang-delete-by-filter.md)
- [{#T}](./sale-status-lang-get-fields.md)

