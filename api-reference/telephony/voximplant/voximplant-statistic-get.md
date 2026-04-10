# Get Call History List voximplant.statistic.get

> Scope: [`telephony`](../../scopes/permissions.md)
>
> Who can execute the method: user with Call Statistics — View permission

The method `voximplant.statistic.get` returns a list of calls from telephony statistics.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **FILTER**
[`object`](../../data-types.md) | An object for filtering in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

See the [list of available fields for filtering](#filterable) below.

Supported operators in the filter key:
- `!` — not equal
- `>=` — greater than or equal
- `>` — greater than
- `<=` — less than or equal
- `<` — less than
- `><` — between (inclusive range)
- `!><` — not between (outside range)
- `?` — string search
- `=` — equal, exact match (used by default)
- `!=` — not equal
- `%` — LIKE, substring search
- `!%` — NOT LIKE, substring search

By default — no filtering
||
|| **SORT**
[`string`](../../data-types.md) | Sorting field.

The same fields as in the [list of fields for filtering](#filterable) are used, except for `CALL_TYPE`.

By default — no sorting ||
|| **ORDER**
[`string`](../../data-types.md) | Sorting direction.

Possible values:
- `ASC` — ascending order
- `DESC` — descending order

By default — no sorting ||
|| **start**
[`integer`](../../data-types.md) | Pagination parameter.

The page size for results is 50 records.

To get the second page, pass `50`; for the third — `100`, and so on.

Formula:

`start = (N - 1) * 50`, where `N` is the page number ||
|#

### Available Fields for Filtering {#filterable}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../../data-types.md) | Internal identifier of the statistics record ||
|| **CALL_ID**
[`string`](../../data-types.md) | Call identifier ||
|| **EXTERNAL_CALL_ID**
[`string`](../../data-types.md) | Call identifier on the external PBX/integration side ||
|| **CALL_CATEGORY**
[`string`](../../data-types.md) | Call category ||
|| **PORTAL_USER_ID**
[`integer`](../../data-types.md) | User identifier.

The identifier can be obtained using the [user.get](../../user/user-get.md) method ||
|| **PORTAL_NUMBER**
[`string`](../../data-types.md) | Line number through which the call was made ||
|| **PHONE_NUMBER**
[`string`](../../data-types.md) | Subscriber number ||
|| **CALL_TYPE**
[`integer`](../../data-types.md) | Type of call.

Possible values:
- `1` — outgoing
- `2` — incoming
- `3` — incoming with redirection
- `4` — callback
- `5` — informational call ||
|| **CALL_DURATION**
[`integer`](../../data-types.md) | Duration of the call in seconds ||
|| **CALL_START_DATE**
[`datetime`](../../data-types.md) | Date and time of the call start in ISO-8601 format with timezone indication ||
|| **CALL_LOG**
[`string`](../../data-types.md) | Call log URL ||
|| **CALL_RECORD_URL**
[`string`](../../data-types.md) | Call recording URL ||
|| **CALL_VOTE**
[`integer`](../../data-types.md) | Call rating.

Possible values:
- `1`, `2`, `3`, `4`, `5`

If the rating is absent — `0` or `null` ||
|| **COST**
[`double`](../../data-types.md) | Cost of the call ||
|| **COST_CURRENCY**
[`string`](../../data-types.md) | Currency of the call cost ||
|| **CALL_FAILED_CODE**
[`string`](../../data-types.md) | Call result code.

Possible values:
- `200` — successful call
- `304` — missed call
- `603` — declined
- `603-S` — call canceled
- `403` — forbidden
- `404` — invalid number
- `486` — busy
- `484` — direction unavailable
- `503` — direction unavailable
- `480` — temporarily unavailable
- `402` — insufficient funds
- `423` — blocked
- `OTHER` — undefined ||
|| **CALL_FAILED_REASON**
[`string`](../../data-types.md) | Text of the reason/result of the call ||
|| **CRM_ENTITY_TYPE**
[`string`](../../data-types.md) | Type of CRM entity.

Possible values:
- `CONTACT` — contact
- `COMPANY` — company
- `LEAD` — lead  ||
|| **CRM_ENTITY_ID**
[`integer`](../../data-types.md) | Identifier of the CRM entity from `CRM_ENTITY_TYPE` ||
|| **CRM_ACTIVITY_ID**
[`integer`](../../data-types.md) | Identifier of the CRM activity for the call ||
|| **REST_APP_ID**
[`integer`](../../data-types.md) | Application identifier ||
|| **REST_APP_NAME**
[`string`](../../data-types.md) | Application name ||
|| **TRANSCRIPT_ID**
[`integer`](../../data-types.md) | Identifier of the call transcript ||
|| **TRANSCRIPT_PENDING**
[`string`](../../data-types.md) | Indicator of pending transcription.

Possible values:
- `Y` — transcription pending
- `N` — transcription available or absent ||
|| **SESSION_ID**
[`integer`](../../data-types.md) | Session identifier on the telephony side ||
|| **REDIAL_ATTEMPT**
[`integer`](../../data-types.md) | Number of redial attempts (for callback scenarios) ||
|| **COMMENT**
[`string`](../../data-types.md) | Comment on the call ||
|| **RECORD_DURATION**
[`integer`](../../data-types.md) | Duration of the call recording file ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"FILTER":{"ID":[1,7],">=CALL_START_DATE":"2025-01-01T00:00:00+01:00"},"SORT":"ID","ORDER":"ASC"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/voximplant.statistic.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"FILTER":{"ID":[1,7],">=CALL_START_DATE":"2025-01-01T00:00:00+01:00"},"SORT":"ID","ORDER":"ASC","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/voximplant.statistic.get
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'voximplant.statistic.get',
            {
                FILTER: {
                    ID: [1, 7],
                    '>=CALL_START_DATE': '2025-01-01T00:00:00+01:00'
                },
                SORT: 'ID',
                ORDER: 'ASC'
            }
        );
        
        const result = response.getData().result;
        console.log('Statistics:', result);
        processResult(result);
    }
    catch( error )
    {
        console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'voximplant.statistic.get',
                [
                    'FILTER' => [
                        'ID' => [1, 7],
                        '>=CALL_START_DATE' => '2025-01-01T00:00:00+01:00'
                    ],
                    'SORT' => 'ID',
                    'ORDER' => 'ASC'
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching statistics: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "voximplant.statistic.get",
        {
            FILTER: {
                ID: [1, 7],
                '>=CALL_START_DATE': '2025-01-01T00:00:00+01:00'
            },
            SORT: 'ID',
            ORDER: 'ASC'
        },
        function(result)
        {
            if (result.error())
            {
                console.error(result.error(), result.error_description());
            }
            else
            {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'voximplant.statistic.get',
        [
            'FILTER' => [
                'ID' => [1, 7],
                '>=CALL_START_DATE' => '2025-01-01T00:00:00+01:00'
            ],
            'SORT' => 'ID',
            'ORDER' => 'ASC'
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
    "result": [
        {
        "ID": "1",
        "PORTAL_USER_ID": "1",
        "PORTAL_NUMBER": "reg133788",
        "PHONE_NUMBER": "+19061234567",
        "CALL_ID": "11018129443EB80D.1754478570.11438214",
        "EXTERNAL_CALL_ID": null,
        "CALL_CATEGORY": "external",
        "CALL_LOG": "https://storage-gw-com-02.voximplant.com/voximplant-logs/2025/08/06/YTdjNmMxYWMyNzNmZDA2NTAwZTlkODYzMWExODN06ODM0MkU1MjY2OEIxMkMuMTc1NDQ3ODUyMC4xMTQzODIxNV8xODUuMTY0LjE0OC4xMzIubG9n?sessionid=3841557776",
        "CALL_DURATION": "0",
        "CALL_START_DATE": "2025-08-06T14:08:40+01:00",
        "CALL_RECORD_URL": "",
        "CALL_VOTE": null,
        "COST": "0.0000",
        "COST_CURRENCY": "EUR",
        "CALL_FAILED_CODE": "603-S",
        "CALL_FAILED_REASON": "Decline self",
        "CRM_ENTITY_TYPE": "CONTACT",
        "CRM_ENTITY_ID": "275",
        "CRM_ACTIVITY_ID": "7739",
        "REST_APP_ID": null,
        "REST_APP_NAME": null,
        "TRANSCRIPT_ID": null,
        "TRANSCRIPT_PENDING": "N",
        "SESSION_ID": "3841557776",
        "REDIAL_ATTEMPT": null,
        "COMMENT": null,
        "RECORD_DURATION": null,
        "RECORD_FILE_ID": null,
        "CALL_TYPE": "1"
        },
        {
        "ID": "7",
        "PORTAL_USER_ID": "1269",
        "PORTAL_NUMBER": "3",
        "PHONE_NUMBER": "19061234568",
        "CALL_ID": "externalCall.716f1cb73def9700a23842adf9c4c568.1773130779",
        "EXTERNAL_CALL_ID": null,
        "CALL_CATEGORY": "external",
        "CALL_LOG": null,
        "CALL_DURATION": "95",
        "CALL_START_DATE": "2026-03-10T11:19:38+01:00",
        "CALL_RECORD_URL": null,
        "CALL_VOTE": "5",
        "COST": "0.0000",
        "COST_CURRENCY": "",
        "CALL_FAILED_CODE": "200",
        "CALL_FAILED_REASON": "",
        "CRM_ENTITY_TYPE": "CONTACT",
        "CRM_ENTITY_ID": "797",
        "CRM_ACTIVITY_ID": "7943",
        "REST_APP_ID": "3",
        "REST_APP_NAME": "REST API Documentation",
        "TRANSCRIPT_ID": "1",
        "TRANSCRIPT_PENDING": "N",
        "SESSION_ID": null,
        "REDIAL_ATTEMPT": null,
        "COMMENT": null,
        "RECORD_DURATION": null,
        "RECORD_FILE_ID": 9079,
        "CALL_TYPE": "2"
        }
    ],
    "total": 2,
    "time": {
        "start": 1773141841,
        "finish": 1773141841.595178,
        "duration": 0.5951778888702393,
        "processing": 0,
        "date_start": "2026-03-10T14:24:01+01:00",
        "date_finish": "2026-03-10T14:24:01+01:00",
        "operating_reset_at": 1773142441,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | Array of statistics records. The composition of records depends on the `FILTER` conditions.

An empty array means there are no records matching the `FILTER` conditions ||
|| **total**
[`integer`](../../data-types.md) | Total number of records in the selection ||
|| **next**
[`integer`](../../data-types.md) | Offset for the next page (if any) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **403**

```json
{
    "error": "ACCESS_DENIED",
    "error_description": "Access denied!"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ACCESS_DENIED` | Access denied! | Insufficient permissions to view call statistics ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](../telephony-external-call-register.md)
- [{#T}](../telephony-external-call-finish.md)
- [{#T}](../telephony-external-call-attach-record.md)
- [{#T}](../telephony-call-attach-transcription.md)