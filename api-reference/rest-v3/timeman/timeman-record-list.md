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

The new API call differs by adding the `/api/` parameter in the request:

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

- JS

    The SDK does not currently support calls to the address /rest/api/. Use direct HTTP requests, such as through curl or fetch.

    ```javascript
    try
    {
        const response = await $b24.callMethod(
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
            }
        );

        const result = response.getData().result;
        console.log('Record list:', result);
    }
    catch( error )
    {
        console.error('Error:', error);
    }
    ```

- PHP

    The SDK does not currently support calls to the address /rest/api/. Use direct HTTP requests, such as through curl or fetch.

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

    The SDK does not currently support calls to the address /rest/api/. Use direct HTTP requests, such as through curl or fetch.

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

    The SDK does not currently support calls to the address /rest/api/. Use direct HTTP requests, such as through curl or fetch.

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