# Get the list of event log entries main.eventlog.list

> Scope: [`main`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `main.eventlog.list` returns a list of event log entries based on specified conditions.

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
- `auditTypeId` — type of event
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
[`array`](../../data-types.md) | Conditions for filtering entries in the format: 
- `["field", "operator", value]`
- `["field", value]`, default operator `=`

Available fields are similar to those in `select`.
  
[More about filtering in REST 3.0](../index.md#filter) ||
|| **order**
[`object`](../../data-types.md) | Sorting results in the format `{ "field": "value" }`.

Available values:
- `ASC` — ascending
- `DESC` — descending

Available fields for sorting are similar to those in `select`. ||
|| **pagination**
[`object`](../../data-types.md) | Pagination parameters:  
- `page` — page number 
- `limit` — number of entries per page, default `50`
- `offset` — record offset. If `page` and `limit` are provided, the offset is calculated automatically ||
|#

## Code Examples

{% include [Footnote about examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` parameter in the request:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/main.eventlog.list`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","timestampX","severity","auditTypeId","moduleId","itemId","userId","description"],"filter":[["timestampX",">=","2026-01-30T00:00:00+02:00"],["timestampX","<","2026-01-31T00:00:00+02:00"]],"order":{"id":"ASC"},"pagination":{"page":1,"limit":20,"offset":0}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/main.eventlog.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","timestampX","severity","auditTypeId","moduleId","itemId","userId","description"],"filter":[["timestampX",">=","2026-01-30T00:00:00+02:00"],["timestampX","<","2026-01-31T00:00:00+02:00"]],"order":{"id":"ASC"},"pagination":{"page":1,"limit":20,"offset":0},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/main.eventlog.list
    ```

- JS

    The SDK does not yet support calls to the address /rest/api/. Use direct HTTP requests, for example, via curl or fetch.

    ```javascript
    try
    {
        const response = await $b24.callMethod(
            'main.eventlog.list',
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
                filter: [
                    ['timestampX', '>=', '2026-01-30T00:00:00+02:00'],
                    ['timestampX', '<', '2026-01-31T00:00:00+02:00']
                ],
                order: {
                    id: 'ASC'
                },
                pagination: {
                    page: 1,
                    limit: 20,
                    offset: 0
                }
            }
        );

        const result = response.getData().result;
        console.log('Event log list:', result);
    }
    catch( error )
    {
        console.error('Error:', error);
    }
    ```

- PHP

    The SDK does not yet support calls to the address /rest/api/. Use direct HTTP requests, for example, via curl or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'main.eventlog.list',
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
                    'filter' => [
                        ['timestampX', '>=', '2026-01-30T00:00:00+02:00'],
                        ['timestampX', '<', '2026-01-31T00:00:00+02:00']
                    ],
                    'order' => [
                        'id' => 'ASC'
                    ],
                    'pagination' => [
                        'page' => 1,
                        'limit' => 20,
                        'offset' => 0
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

    The SDK does not yet support calls to the address /rest/api/. Use direct HTTP requests, for example, via curl or fetch.

    ```js
    BX24.callMethod(
        'main.eventlog.list',
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
            filter: [
                ['timestampX', '>=', '2026-01-30T00:00:00+02:00'],
                ['timestampX', '<', '2026-01-31T00:00:00+02:00']
            ],
            order: {
                id: 'ASC'
            },
            pagination: {
                page: 1,
                limit: 20,
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

    The SDK does not yet support calls to the address /rest/api/. Use direct HTTP requests, for example, via curl or fetch.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'main.eventlog.list',
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
            'filter' => [
                ['timestampX', '>=', '2026-01-30T00:00:00+02:00'],
                ['timestampX', '<', '2026-01-31T00:00:00+02:00']
            ],
            'order' => [
                'id' => 'ASC'
            ],
            'pagination' => [
                'page' => 1,
                'limit' => 20,
                'offset' => 0
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
                "id": 443585,
                "timestampX": "2026-01-22T02:52:25+02:00",
                "severity": "SECURITY",
                "auditTypeId": "USER_AUTHORIZE",
                "moduleId": "main",
                "itemId": "751",
                "userId": 751,
                "description": "{\u0022userId\u0022:751,\u0022requestId\u0022:\u00225636cd3e45c524c55a68a19dccb72c3b-0\u0022,\u0022method\u0022:\u0022external\u0022}"
            },
            // ...
            {
                "id": 443623,
                "timestampX": "2026-01-22T05:21:51+02:00",
                "severity": "SECURITY",
                "auditTypeId": "USER_AUTHORIZE",
                "moduleId": "main",
                "itemId": "751",
                "userId": 751,
                "description": "{\u0022userId\u0022:751,\u0022requestId\u0022:\u002239aae4ca278ad75ca3dc631c7b4f8fe2-0\u0022,\u0022method\u0022:\u0022external\u0022}"
            }
        ]
    },
    "time": {
        "start": 1769773532,
        "finish": 1769773532.960023,
        "duration": 0.9600229263305664,
        "processing": 0,
        "date_start": "2026-01-30T14:45:32+02:00",
        "date_finish": "2026-01-30T14:45:32+02:00",
        "operating_reset_at": 1769774132,
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
[`string`](../../data-types.md) | Type of event ||
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
        "code": "BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION",
        "message": "Cannot recognize pagination parameter `{\"limit\":\"abc\"}`"
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

#### Pagination Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `limit`
`offset`
`page` | Cannot recognize pagination parameter `#PAGE#` | Provide numeric values. `limit` must not be equal to `0` ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./main-eventlog-get.md)
- [{#T}](./main-eventlog-tail.md)
- [{#T}](./index.md)