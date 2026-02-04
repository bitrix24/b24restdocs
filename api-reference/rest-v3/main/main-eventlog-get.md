# Get Event Log Entry main.eventlog.get

> Scope: [`main`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `main.eventlog.get` returns an event log entry by its identifier.

## Method Parameters

{% include [Footnote about parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Identifier of the log entry ||
|| **select**
[`array`](../../data-types.md) | List of fields to return in the response.

Available fields:
- `id` — identifier of the log entry
- `timestampX` — date and time of the event
- `severity` — importance level of the event
- `auditTypeId` — type of event
- `moduleId` — module identifier
- `itemId` — object identifier
- `remoteAddr` — IP address
- `userAgent` — User-Agent of the request
- `requestUri` — request URI
- `siteId` — site identifier
- `userId` — user identifier
- `guestId` — guest identifier
- `description` — event description ||
|#

## Code Examples

{% include [Footnote about examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the parameter `/api/` in the request:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/main.eventlog.get`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1250}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/main.eventlog.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1250,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/main.eventlog.get
    ```

- JS

    The SDK does not currently support the address /rest/api/ in calls. Use direct HTTP requests, for example, via curl or fetch.

    ```javascript
    try
    {
        const response = await $b24.callMethod(
            'main.eventlog.get',
            {
                id: 1250
            }
        );

        const result = response.getData().result;
        console.log('Event log item:', result);
    }
    catch( error )
    {
        console.error('Error:', error);
    }
    ```

- PHP

    The SDK does not currently support the address /rest/api/ in calls. Use direct HTTP requests, for example, via curl or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'main.eventlog.get',
                [
                    'id' => 1250
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

    The SDK does not currently support the address /rest/api/ in calls. Use direct HTTP requests, for example, via curl or fetch.

    ```js
    BX24.callMethod(
        'main.eventlog.get',
        {
            id: 1250
        },
        function(result){
            console.info(result.data());
            console.log(result);
        }
    );
    ```

- PHP CRest

    The SDK does not currently support the address /rest/api/ in calls. Use direct HTTP requests, for example, via curl or fetch.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'main.eventlog.get',
        [
            'id' => 1250
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
        "item": {
            "id": 839,
            "timestampX": "2026-01-30T15:50:24+03:00",
            "severity": "SECURITY",
            "auditTypeId": "USER_AUTHORIZE",
            "moduleId": "main",
            "itemId": "1",
            "remoteAddr": "3.75.32.66",
            "userAgent": "Mozilla\/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit\/537.36 (KHTML, like Gecko) Chrome\/144.0.0.0 Safari\/537.36",
            "requestUri": "\/bitrix\/tools\/oauth\/bitrix24net.php?code=2da97c6900ddfdfd01b8d0b200000000000000eda156e035f598dcc2f2927de6c527dc\u0026state=s1.0.79b6f9ab2822c2482d1b15948602b656e8a1722ce4a4ff265297d59d\u0026domain=www.bitrix24.net\u0026user_lang=en\u0026analytics_session_id=pru9c9jdehq31u2v",
            "siteId": "s1",
            "userId": 1,
            "guestId": 0,
            "description": "{\u0022userId\u0022:1,\u0022requestId\u0022:\u00220fbda8a91117ccbb8e2205446c18cac9\\\u0022-\\\u00220\u0022,\u0022method\u0022:\u0022external\u0022,\u0022externalAuthId\u0022:\u0022Bitrix24Net\u0022,\u0022externalId\u0022:\u002228889266\u0022}"
        }
    },
    "time": {
        "start": 1769777553,
        "finish": 1769777553.749127,
        "duration": 0.7491269111633301,
        "processing": 0,
        "date_start": "2026-01-30T15:52:33+03:00",
        "date_finish": "2026-01-30T15:52:33+03:00",
        "operating_reset_at": 1769778153,
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
|| **item**
[`object`](../../data-types.md) | Log entry object ||
|| **id**
[`integer`](../../data-types.md) | Identifier of the log entry ||
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
[`string`](../../data-types.md) | User-Agent of the request ||
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
        "code": "BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION",
        "message": "Entry with ID = `999999` not found"
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

#### Data Not Found Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `id` | Entry with ID = `#ID#` not found | Specify `id` of an existing log entry ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./main-eventlog-list.md)
- [{#T}](./main-eventlog-tail.md)
- [{#T}](./index.md)