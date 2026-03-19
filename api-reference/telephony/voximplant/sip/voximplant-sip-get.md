# Get SIP Lines of the Application voximplant.sip.get

> Scope: [`telephony`](../../../scopes/permissions.md)
>
> Who can execute the method: user with the Management of Numbers — modification access permission

The method `voximplant.sip.get` returns a list of SIP lines created by the current application.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **FILTER**
[`object`](../../../data-types.md) | Object for filtering in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

See the [list of available fields for filtering](#filterable) below.

Supported operators in the filter key:
- `!` — not equal
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `><` — between (inclusive range)
- `!><` — not between (outside the range)
- `?` — string search
- `=` — equal, exact match (used by default)
- `!=` — not equal
- `%` — LIKE, substring search
- `!%` — NOT LIKE, substring search

By default — no filtering.

The method always adds a system filter by `APP_ID` of the current application. ||
|| **SORT**
[`string`](../../../data-types.md) | Sorting field.

Uses fields from the [list of fields for filtering](#filterable).

By default — no sorting. ||
|| **ORDER**
[`string`](../../../data-types.md) | Sorting direction.

Possible values:
- `ASC` — ascending order
- `DESC` — descending order

By default — no sorting. ||
|| **start**
[`integer`](../../../data-types.md) | Pagination parameter.

The page size of results is 50 records.

To get the second page, pass `50`; for the third — `100`, and so on.

Formula:

`start = (N - 1) * 50`, where `N` is the page number. ||
|#

### Available Fields for Filtering {#filterable}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../../../data-types.md) | Internal identifier of the SIP line record.

This field is not returned in the method response. ||
|| **TYPE**
[`string`](../../../data-types.md) | Type of PBX.

Possible values:

- `cloud` — cloud PBX
- `office` — office PBX ||
|| **TITLE**
[`string`](../../../data-types.md) | Connection name. ||
|| **CONFIG_ID**
[`integer`](../../../data-types.md) | Identifier of the SIP line configuration. ||
|| **REG_ID**
[`integer`](../../../data-types.md) | Identifier of the SIP registration.

Relevant for cloud PBX. ||
|| **APP_ID**
[`string`](../../../data-types.md) | Application identifier.

Filtering by this field is forcibly limited to the current application. ||
|| **SERVER**
[`string`](../../../data-types.md) | Address of the SIP registration server. ||
|| **LOGIN**
[`string`](../../../data-types.md) | Login for connecting to the server. ||
|| **PASSWORD**
[`string`](../../../data-types.md) | Password for connecting to the server. ||
|| **INCOMING_SERVER**
[`string`](../../../data-types.md) | Address of the server for incoming calls.

Relevant for office PBX. ||
|| **INCOMING_LOGIN**
[`string`](../../../data-types.md) | Login for incoming calls.

Relevant for office PBX. ||
|| **INCOMING_PASSWORD**
[`string`](../../../data-types.md) | Password for incoming calls.

Relevant for office PBX. ||
|| **AUTH_USER**
[`string`](../../../data-types.md) | Username for authentication. ||
|| **OUTBOUND_PROXY**
[`string`](../../../data-types.md) | Address of the SIP proxy for outgoing connection to the operator or PBX. ||
|| **DETECT_LINE_NUMBER**
[`string`](../../../data-types.md) | Indicator for line number detection.

Possible values:

- `Y` — line number detection enabled
- `N` — line number detection disabled. ||
|| **LINE_DETECT_HEADER_ORDER**
[`string`](../../../data-types.md) | Order of headers for line number detection. ||
|| **REGISTRATION_STATUS_CODE**
[`integer`](../../../data-types.md) | SIP registration status code. ||
|| **REGISTRATION_ERROR_MESSAGE**
[`string`](../../../data-types.md) | SIP registration error message. ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"FILTER":{"CONFIG_ID":[3,7,9]},"SORT":"CONFIG_ID","ORDER":"ASC"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/voximplant.sip.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"FILTER":{"CONFIG_ID":[3,7,9]},"SORT":"CONFIG_ID","ORDER":"ASC","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/voximplant.sip.get
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'voximplant.sip.get',
            {
                FILTER: {
                    CONFIG_ID: [3, 7, 9]
                },
                SORT: 'CONFIG_ID',
                ORDER: 'ASC'
            }
        );
        
        const result = response.getData().result;
        console.log('Fetched SIP data:', result);
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
                'voximplant.sip.get',
                [
                    'FILTER' => [
                        'CONFIG_ID' => [3, 7, 9]
                    ],
                    'SORT' => 'CONFIG_ID',
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
        echo 'Error fetching SIP data: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'voximplant.sip.get',
        {
            FILTER: {
                CONFIG_ID: [3, 7, 9]
            },
            SORT: 'CONFIG_ID',
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
        'voximplant.sip.get',
        [
            'FILTER' => [
                'CONFIG_ID' => [3, 7, 9]
            ],
            'SORT' => 'CONFIG_ID',
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
        "TYPE": "cloud",
        "CONFIG_ID": "3",
        "REG_ID": "150907",
        "SERVER": "sipnet.com",
        "LOGIN": "0045281811",
        "PASSWORD": "N7K4mCG9",
        "AUTH_USER": "",
        "OUTBOUND_PROXY": "",
        "DETECT_LINE_NUMBER": "N",
        "LINE_DETECT_HEADER_ORDER": "diversion;to",
        "REGISTRATION_STATUS_CODE": "200",
        "REGISTRATION_ERROR_MESSAGE": "",
        "TITLE": "SIP line"
        },
        {
        "TYPE": "office",
        "CONFIG_ID": "7",
        "SERVER": "office.provider.local",
        "LOGIN": "office_user",
        "PASSWORD": "secret",
        "INCOMING_SERVER": "ip.b24-6068-1537535782.bitrixphone.com",
        "INCOMING_LOGIN": "sip7",
        "INCOMING_PASSWORD": "71747523265mb091225eb31996a4a225",
        "AUTH_USER": null,
        "OUTBOUND_PROXY": null,
        "DETECT_LINE_NUMBER": "N",
        "LINE_DETECT_HEADER_ORDER": "diversion;to",
        "REGISTRATION_STATUS_CODE": "0",
        "REGISTRATION_ERROR_MESSAGE": null,
        "TITLE": "Office PBX 1"
        },
        {
        "TYPE": "cloud",
        "CONFIG_ID": "9",
        "REG_ID": "151083",
        "SERVER": "sip.provider.com",
        "LOGIN": "sip_user",
        "PASSWORD": "secret",
        "AUTH_USER": null,
        "OUTBOUND_PROXY": null,
        "DETECT_LINE_NUMBER": "N",
        "LINE_DETECT_HEADER_ORDER": "diversion;to",
        "REGISTRATION_STATUS_CODE": "502",
        "REGISTRATION_ERROR_MESSAGE": "Unable to resolve hostname",
        "TITLE": ""
        }
    ],
    "total": 3,
    "time": {
        "start": 1773662224,
        "finish": 1773662224.187874,
        "duration": 0.18787407875061035,
        "processing": 0,
        "date_start": "2026-03-16T14:57:04+01:00",
        "date_finish": "2026-03-16T14:57:04+01:00",
        "operating_reset_at": 1773662824,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object[]`](../../../data-types.md) | Array of SIP line records created by the current application. The composition of records depends on the `FILTER` conditions.

An empty array means that there are no records matching the `FILTER` conditions. ||
|| **total**
[`integer`](../../../data-types.md) | Total number of records in the selection. ||
|| **next**
[`integer`](../../../data-types.md) | Offset for the next page (if any). ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request. ||
|#

## Error Handling

HTTP Status: **403**

```json
{
    "error": "ACCESS_DENIED",
    "error_description": "Access denied!"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ACCESS_DENIED` | `Access denied!` | Insufficient permissions to retrieve the list of SIP lines. ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./voximplant-sip-add.md)
- [{#T}](./voximplant-sip-update.md)
- [{#T}](./voximplant-sip-get.md)
- [{#T}](./voximplant-sip-delete.md)
- [{#T}](./voximplant-sip-status.md)
- [{#T}](./voximplant-sip-connector-status.md)