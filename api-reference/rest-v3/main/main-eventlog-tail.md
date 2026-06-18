# Get New Log Entries main.eventlog.tail

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`main`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `main.eventlog.tail` returns new log entries that appeared after the specified `cursor` reference point, considering the filter.

## Method Parameters

{% include [Footnote about parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | List of fields to return in the response.

Available fields:
- `id` — log entry identifier
- `timestampX` — date and time of the event
- `severity` — importance level of the event
- `auditTypeId` — event type
- `moduleId` — module identifier
- `itemId` — object identifier
- `remoteAddr` — IP address
- `userAgent` — request User-Agent
- `requestUri` — request URI
- `siteId` — site identifier
- `userId` — user identifier
- `guestId` — guest identifier
- `description` — event description ||
|| **filter**
[`array`](../../data-types.md) | Conditions for filtering records in the format: 
- `["field", "operator", value]`
- `["field", value]`, default operator `=`

Available fields are similar to those in `select`.
  
[More about filtering in REST 3.0](../index.md#filter) ||
|| **cursor**
[`object`](../../data-types.md) | Reference point for retrieving new records: 

- `field` — sorting field, default `id`
- `order` — sorting direction `ASC` or `DESC`, default `ASC`
- `value` — starting value, default `0`
- `limit` — record limit, default `50`
||
|#

## Code Examples

{% include [Footnote about examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/main.eventlog.tail`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","timestampX","severity","auditTypeId","moduleId","itemId","userId","description"],"filter":[],"cursor":{"field":"id","value":446313,"order":"ASC"}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/main.eventlog.tail
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","timestampX","severity","auditTypeId","moduleId","itemId","userId","description"],"filter":[],"cursor":{"field":"id","value":446313,"order":"ASC"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/main.eventlog.tail
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type EventLogEntry = {
      id: number
      timestampX: ISODate
      severity: string
      auditTypeId: string
      moduleId: string
      itemId: string
      remoteAddr: string
      userAgent: string
      requestUri: string
      siteId: string
      userId: number
      guestId: number
      description: string
    }

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type EventLogTailResult = {
      items: EventLogEntry[]
    }

    try {
      const response = await $b24.actions.v3.call.make<EventLogTailResult>({
        method: 'main.eventlog.tail',
        params: {
          select: [
            'id',
            'timestampX',
            'severity',
            'auditTypeId',
            'moduleId',
            'itemId',
            'userId',
            'description',
          ],
          filter: [],
          cursor: {
            field: 'id',
            value: 446313,
            order: 'ASC',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Received entries:', result.items.length, result.items[0]?.id)
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
      async function fetchEventLogTail() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'main.eventlog.tail',
            params: {
              select: [
                'id',
                'timestampX',
                'severity',
                'auditTypeId',
                'moduleId',
                'itemId',
                'userId',
                'description',
              ],
              filter: [],
              cursor: {
                field: 'id',
                value: 446313,
                order: 'ASC',
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Received entries:', result.items.length, result.items[0]?.id)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', fetchEventLogTail)
    </script>
    ```

- PHP

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'main.eventlog.tail',
                [
                    'select' => [
                        'id',
                        'timestampX',
                        'severity',
                        'auditTypeId',
                        'moduleId',
                        'itemId',
                        'userId',
                        'description'
                    ],
                    'filter' => [],
                    'cursor' => [
                        'field' => 'id',
                        'value' => 446313,
                        'order' => 'ASC'
                    ]
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```js
    BX24.callMethod(
        'main.eventlog.tail',
        {
            select: [
                'id',
                'timestampX',
                'severity',
                'auditTypeId',
                'moduleId',
                'itemId',
                'userId',
                'description'
            ],
            filter: [],
            cursor: {
                field: 'id',
                value: 446313,
                order: 'ASC'
            }
        },
        function(result){
            console.info(result.data());
            console.log(result);
        }
    );
    ```

- PHP CRest

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'main.eventlog.tail',
        [
            'select' => [
                'id',
                'timestampX',
                'severity',
                'auditTypeId',
                'moduleId',
                'itemId',
                'userId',
                'description'
            ],
            'filter' => [],
            'cursor' => [
                'field' => 'id',
                'value' => 446313,
                'order' => 'ASC'
            ]
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
        "items": [
            {
                "id": 446315,
                "timestampX": "2026-01-30T14:05:38+02:00",
                "severity": "SECURITY",
                "auditTypeId": "USER_AUTHORIZE",
                "moduleId": "main",
                "itemId": "1463",
                "userId": 1463,
                "description": "{\"userId\":1463,\"applicationId\":\"mobile\",\"applicationPasswordId\":977,\"requestId\":\"1649cc2c8540e12c1f3aeb1e670c13f8\\\"-\\\"0\",\"method\":\"appPassword\"}"
            },
            // ....
            {
                "id": 446325,
                "timestampX": "2026-01-30T15:03:02+02:00",
                "severity": "SECURITY",
                "auditTypeId": "USER_AUTHORIZE",
                "moduleId": "rest",
                "itemId": "971",
                "userId": 971,
                "description": "{\"userId\":971,\"method\":\"rest\",\"applicationType\":\"apauth\",\"applicationId\":383,\"timePeriod\":\"2026-01-30 15\"}"
            }
        ]
    },
    "time": {
        "start": 1769774582,
        "finish": 1769774583.014603,
        "duration": 1.0146028995513916,
        "processing": 0,
        "date_start": "2026-01-30T15:03:02+02:00",
        "date_finish": "2026-01-30T15:03:03+02:00",
        "operating_reset_at": 1769775183,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Object with response data ||
|| **items**
[`array`](../../data-types.md) | Array of log entry objects ||
|| **items[]**
[`object`](../../data-types.md) | Log entry object ||
|| **id**
[`integer`](../../data-types.md) | Log entry identifier ||
|| **timestampX**
[`datetime`](../../data-types.md#datetime) | Date and time of the event ||
|| **severity**
[`string`](../../data-types.md) | Importance level of the event ||
|| **auditTypeId**
[`string`](../../data-types.md) | Event type ||
|| **moduleId**
[`string`](../../data-types.md) | Module identifier ||
|| **itemId**
[`string`](../../data-types.md) | Object identifier ||
|| **remoteAddr**
[`string`](../../data-types.md) | IP address ||
|| **userAgent**
[`string`](../../data-types.md) | Request User-Agent ||
|| **requestUri**
[`string`](../../data-types.md) | Request URI ||
|| **siteId**
[`string`](../../data-types.md) | Site identifier ||
|| **userId**
[`integer`](../../data-types.md) | User identifier ||
|| **guestId**
[`integer`](../../data-types.md) | Guest identifier ||
|| **description**
[`string`](../../data-types.md) | Event description ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_INVALIDFILTEREXCEPTION",
        "message": "Unable to recognize filter expression `Cursor field id cannot be used at filter.`"
    }
}
```

{% include notitle [error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Access Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Access denied | No administrator rights or missing scope main ||
|#

#### Filtering Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDFILTEREXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `filter` | Unable to recognize filter expression `#FILTER#` | Remove the field specified in `cursor.field` from `filter` ||
|#

#### Pagination Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `cursor.limit` | Unable to recognize pagination parameter `#PAGE#` | Provide a numeric `limit` greater than `0` ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./main-eventlog-list.md)
- [{#T}](./main-eventlog-get.md)
- [{#T}](./index.md)
