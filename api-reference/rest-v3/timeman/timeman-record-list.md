# Get a List of Time Records timeman.record.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`timeman`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "Read according to subordination settings" or "Read all records" access permission.

The method `timeman.record.list` returns a list of time records for an employee.

## Method Parameters

{% include [Note on Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **filter***
[`array`](../../data-types.md) | Conditions for filtering records in the format:
- `["field", "operator", value]`
- `["field", value]`, default operator is `=`

The `userId` parameter is required.

Available fields:
- `userId` — employee identifier
- `startTime` — start time of the work interval

For `startTime`, the following operators are supported: `=`, `>`, `>=`, `<`, `<=`, `between`.

[More about filtering in REST 3.0](../index.md#filter) ||
|| **select**
[`array`](../../data-types.md) | List of fields to return in the response.

Available fields:
- `id` — record identifier
- `userId` — employee identifier
- `startTime` — start time of the work interval
- `endTime` — end time of the work interval
- `duration` — duration of the work interval in seconds
- `breakLength` — duration of breaks in seconds
- `state` — state of the record
- `isApproved` — approval status of the record ||
|| **order**
[`object`](../../data-types.md) | Sorting results in the format `{ "field": "value" }`.

Available values:
- `ASC` — ascending
- `DESC` — descending

Available fields for sorting:
- `id` — record identifier
- `userId` — employee identifier
- `startTime` — start time of the work interval
- `endTime` — end time of the work interval
- `duration` — duration of the work interval in seconds ||
|| **pagination**
[`object`](../../data-types.md) | Pagination parameters:
- `page` — page number
- `limit` — number of records per page, default is `50`
- `offset` — record offset. If `page` and `limit` are provided, the offset is calculated automatically ||
|#

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/timeman.record.list`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":[["userId",1],["startTime","between",["2026-06-01T00:00:00+02:00","2026-06-30T23:59:59+02:00"]]],"select":["id","startTime","endTime","duration"],"order":{"startTime":"DESC"},"pagination":{"page":1,"limit":50,"offset":0}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/timeman.record.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":[["userId",1],["startTime","between",["2026-06-01T00:00:00+02:00","2026-06-30T23:59:59+02:00"]]],"select":["id","startTime","endTime","duration"],"order":{"startTime":"DESC"},"pagination":{"page":1,"limit":50,"offset":0},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/timeman.record.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type TimemanRecordItem = {
      id: number
      startTime: ISODate
      endTime: ISODate
      duration: number
    }

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type TimemanRecordListResult = {
      items: TimemanRecordItem[]
    }

    try {
      // timeman.record.list returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `pagination` variant when sort matters.
      const response = await $b24.actions.v3.call.make<TimemanRecordListResult>({
        method: 'timeman.record.list',
        params: {
          filter: [
            ['userId', 1],
            ['startTime', 'between', ['2026-06-01T00:00:00+03:00', '2026-06-30T23:59:59+03:00']],
          ],
          select: [
            'id',
            'startTime',
            'endTime',
            'duration',
          ],
          order: {
            startTime: 'DESC',
          },
          pagination: {
            page: 1,
            limit: 50,
            offset: 0,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Records fetched:', result.items.length, result.items)
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
      async function fetchTimemanRecordList() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // timeman.record.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `pagination` variant when sort matters.
          const response = await $b24.actions.v3.call.make({
            method: 'timeman.record.list',
            params: {
              filter: [
                ['userId', 1],
                ['startTime', 'between', ['2026-06-01T00:00:00+03:00', '2026-06-30T23:59:59+03:00']],
              ],
              select: [
                'id',
                'startTime',
                'endTime',
                'duration',
              ],
              order: {
                startTime: 'DESC',
              },
              pagination: {
                page: 1,
                limit: 50,
                offset: 0,
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
          console.info('Records fetched:', result.items.length, result.items)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', fetchTimemanRecordList)
    </script>
    ```

- PHP

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'timeman.record.list',
                [
                    'filter' => [
                        ['userId', 1],
                        ['startTime', 'between', ['2026-06-01T00:00:00+02:00', '2026-06-30T23:59:59+02:00']],
                    ],
                    'select' => [
                        'id',
                        'startTime',
                        'endTime',
                        'duration'
                    ],
                    'order' => [
                        'startTime' => 'DESC'
                    ],
                    'pagination' => [
                        'page' => 1, 
                        'limit' => 50, 
                        'offset' => 0
                    ],
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
        'timeman.record.list',
        {
            filter: [
                ['userId', 1],
                ['startTime', 'between', ['2026-06-01T00:00:00+02:00', '2026-06-30T23:59:59+02:00']]
            ],
            select: [
                'id',
                'startTime', 
                'endTime',
                'duration'
                ],
            order: {
                 startTime: 'DESC'
                },
            pagination: { 
                page: 1,
                limit: 50, 
                offset: 0 
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
        'timeman.record.list',
        [
            'filter' => [
                ['userId', 1],
                ['startTime', 'between', ['2026-06-01T00:00:00+02:00', '2026-06-30T23:59:59+02:00']]
            ],
            'select' => [
                'id',
                'startTime',
                'endTime',
                'duration'
            ],
            'order' => [
                'startTime' => 'DESC'
            ],
            'pagination' => [
                'page' => 1, 
                'limit' => 50, 
                'offset' => 0
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
        "items": [
            {
                "id": 177,
                "startTime": "2026-06-02T17:38:24+02:00",
                "endTime": "2026-06-02T17:39:06+02:00",
                "duration": 28
            }
        ]
    },
    "time": {
        "start": 1780411219,
        "finish": 1780411219.167315,
        "duration": 0.16731500625610352,
        "processing": 0,
        "date_start": "2026-06-02T17:40:19+02:00",
        "date_finish": "2026-06-02T17:40:19+02:00",
        "operating_reset_at": 1780411819,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Object containing response data ||
|| **items**
[`array`](../../data-types.md) | Array of objects with records. The response structure depends on `select` ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION",
        "message": "Error during request object validation",
        "validation": [
            {
                "field": "filter.userId",
                "message": "Required field `filter.userId` is not specified"
            }
        ]
    }
}
```

{% include notitle [error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Access Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `filter.userId` | Access denied | Check the read permission for time records for the specified employee and the `timeman` scope ||
|#

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `filter.userId` | Required field `filter.userId` is not specified | Provide a filter with a condition for `userId` ||
|#

#### Filter Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDFILTEREXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `filter.userId` | Multiple different `userId` values were provided in one request | Only provide one employee identifier in the filter ||
|#

#### Pagination Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `limit`
`offset`
`page` | Unable to recognize pagination parameter `#PAGE#` | Provide numeric values. `limit` must not be equal to `0` ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./timeman-record-field-list.md)
- [{#T}](./timeman-record-field-get.md)
- [{#T}](./index.md)
