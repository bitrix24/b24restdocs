# Add Absence Report timeman.timecontrol.report.add

> Scope: [`timeman`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `timeman.timecontrol.report.add` sends an absence report and adds it to the calendar.

By default, a user can only send a report for themselves. A portal administrator can send a report for anyone—this requires specifying the user ID in the `USER_ID` parameter.

## Method Parameters

{% include [Footnote about parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **REPORT_ID*** \| **ID***
[`integer`](../../data-types.md) | Identifier of the absence record.

You can obtain record IDs using the [timeman.timecontrol.reports.get](./timeman-timecontrol-reports-get.md#reports) method ||
|| **USER_ID**
[`integer`](../../data-types.md) | User identifier. Can only be specified by an administrator.

You can obtain the user ID using the [user.get](../../user/user-get.md) method ||
|| **TEXT***
[`string`](../../data-types.md) | Report text ||
|| **TYPE**
[`string`](../../data-types.md) | Report type:
- `WORK` — work-related
- `PRIVATE` — personal

Default value is `PRIVATE` ||
|| **CALENDAR**
[`string`](../../data-types.md) | Add event to calendar:
- `Y` — yes
- `N` — no

Default value is `Y` ||
|#

## Code Examples

{% include [Footnote about examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"REPORT_ID":123,"TEXT":"Worked on the project","TYPE":"WORK","CALENDAR":"Y"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/timeman.timecontrol.report.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"REPORT_ID":123,"TEXT":"Worked on the project","TYPE":"WORK","CALENDAR":"Y","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/timeman.timecontrol.report.add
    ```

- JS

    ```js
    BX24.callMethod(
        'timeman.timecontrol.report.add',
        {
            'REPORT_ID': 123,
            'TEXT': 'Worked on the project',
            'TYPE': 'WORK',
            'CALENDAR': 'Y'
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

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'timeman.timecontrol.report.add',
        [
            'REPORT_ID' => 123,
            'TEXT' => 'Worked on the project',
            'TYPE' => 'WORK',
            'CALENDAR' => 'Y'
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
    "result": true,
    "time": {
        "start": 1743056587.6559751,
        "finish": 1743056587.8529301,
        "duration": 0.19695496559143066,
        "processing": 0.16714906692504883,
        "date_start": "2025-03-27T09:23:07+02:00",
        "date_finish": "2025-03-27T09:23:07+02:00",
        "operating_reset_at": 1743057187,
        "operating": 0.1671299934387207
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Execution result. Returns `true` if the report was added successfully ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "TEXT_EMPTY",
    "error_description": "Text can't be empty"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ACCESS_ERROR` | You don't have access for this report | You do not have access to this report ||
|| `TEXT_EMPTY` | Text can't be empty | The report text cannot be empty ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./timeman-timecontrol-reports-get.md)
- [{#T}](./timeman-timecontrol-reports-users-get.md)