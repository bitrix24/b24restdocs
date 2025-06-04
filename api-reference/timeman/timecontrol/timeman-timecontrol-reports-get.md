# Get Report on Identified Absences timeman.timecontrol.reports.get

> Scope: [`timeman`](../../scopes/permissions.md)
>
> Who can execute the method: any user with report viewing access

The method `timeman.timecontrol.reports.get` retrieves a report on identified absences.

## Method Parameters

{% include [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **USER_ID***
[`integer`](../../data-types.md) | User ID for whom the reports are requested.

You can obtain the user ID using the [user.get](../../user/user-get.md) method. ||
|| **MONTH***
[`integer`](../../data-types.md) | Month number ||
|| **YEAR***
[`integer`](../../data-types.md) | Year ||
|| **IDLE_MINUTES**
[`integer`](../../data-types.md) | Maximum time of absence at the workplace that is not counted as absence.

This parameter is available to the manager and administrator.

If not specified, the time from the module settings is used. ||
|| **WORKDAY_HOURS**
[`integer`](../../data-types.md) | Duration of the workday in hours.

Default is 8 hours. ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"USER_ID":3,"MONTH":5,"YEAR":2025,"IDLE_MINUTES":15,"WORKDAY_HOURS":8}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/timeman.timecontrol.reports.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"USER_ID":3,"MONTH":5,"YEAR":2025,"IDLE_MINUTES":15,"WORKDAY_HOURS":8,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/timeman.timecontrol.reports.get
    ```

- JS

    ```js
    BX24.callMethod(
        'timeman.timecontrol.reports.get',
        {
            'USER_ID': 3,
            'MONTH': 5,
            'YEAR': 2025,
            'IDLE_MINUTES': 15,
            'WORKDAY_HOURS': 8
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
        'timeman.timecontrol.reports.get',
        [
            'USER_ID' => 3,
            'MONTH' => 5,
            'YEAR' => 2025,
            'IDLE_MINUTES' => 15,
            'WORKDAY_HOURS' => 8
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
        "report": {
            "month_title": "May",
            "date_start": "2025-05-01T00:00:00+02:00",
            "date_finish": "2025-05-31T23:59:59+02:00",
            "days": [
                {
                    "index": "20250526",
                    "day_title": "05/26/2025",
                    "workday_date_start": "2025-05-26T14:44:47+02:00",
                    "workday_date_finish": "2025-05-26T14:45:29+02:00",
                    "workday_complete": true,
                    "workday_time_leaks_user": 0,
                    "workday_time_leaks_final": 28758,
                    "workday_duration": 42,
                    "workday_duration_final": 42,
                    "workday_duration_config": 28800,
                    "reports": [
                        {
                            "id": "27",
                            "user_id": "503",
                            "type": "TM_START",
                            "date_start": "2025-05-26T14:44:47+02:00",
                            "date_finish": "2025-05-26T14:44:47+02:00",
                            "duration": 0,
                            "active": false,
                            "entry_id": "2237",
                            "report_type": "WORK",
                            "report_text": "Worked on the project",
                            "system_text": null,
                            "source_start": "TM_EVENT",
                            "source_finish": "TM_EVENT",
                            "ip_start": "83.219.151.30",
                            "ip_finish": "83.219.151.30",
                            "ip_start_network": false,
                            "ip_finish_network": false
                        },
                        {
                            "id": "29",
                            ...
                        }
                    ],
                    "workday_time_leaks_real": 0
                }
            ]
        },
        "user": {
            "id": 3,
            "active": true,
            "name": "Maria Ivshina",
            "first_name": "Maria",
            "last_name": "Ivshina",
            "work_position": "IT Specialist",
            "avatar": "http://test.bitrix24.com/upload/resize_cache/45749/7acf4ca766af5d8/main/c89/c89c6b73470635c/4R5A1256.png",
            "personal_gender": "F",
            "last_activity_date": "2025-05-29T17:15:56+02:00"
        },
        "time": {
            "start": 1748528193.688745,
            "finish": 1748528193.730104,
            "duration": 0.04135894775390625,
            "processing": 0.014277935028076172,
            "date_start": "2025-05-29T17:16:33+02:00",
            "date_finish": "2025-05-29T17:16:33+02:00",
            "operating_reset_at": 1748528793,
            "operating": 0
        }
    }
}
```

### If the response contains empty days

If the method returns an empty array `days`, configure the time control tool.
1. Execute the method [timeman.timecontrol.settings.set](./timeman-timecontrol-settings-set.md) under an administrator with the following parameters:

    ```js
    BX24.callMethod(
        'timeman.timecontrol.settings.set',
        {
            active: true,
            REPORT_SIMPLE_TYPE: 'all',
            REPORT_FULL_TYPE: 'all',
            report_request_type: 'user',
            report_request_users: 3,
        },
        function(result){
            if(result.error())
            {
                console.error(result.error().ex);
            }
            else
            {
                console.log(result.data());
            }
        }
    );
    ```

2. Open or close the user's workday.
3. Execute the method `timeman.timecontrol.reports.get`. The response will include data in `days`.

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **report**
[`object`](../../data-types.md) | Report information ||
|| **month_title**
[`string`](../../data-types.md) | Month name ||
|| **date_start**
[`datetime`](../../data-types.md) | Start date of the sampling period in ATOM format ||
|| **date_finish**
[`datetime`](../../data-types.md) | End date of the sampling period in ATOM format ||
|| **days**
[`array`](../../data-types.md) | List of objects describing [worked days](#days) ||
|| **user**
[`object`](../../data-types.md) | Object with information about the [user](#user) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

### Objects days {#days}

#|
|| **Name**
`type` | **Description** ||
|| **index**
[`integer`](../../data-types.md) | Index of the day of the week in the format `YYYYMMDD`, for example, `20250526` for May 26, 2025 ||
|| **day_title**
[`string`](../../data-types.md) | Date in site format ||
|| **workday_date_start**
[`datetime`](../../data-types.md) | Start date of the workday in ATOM format ||
|| **workday_date_finish**
[`datetime`](../../data-types.md) | End date of the workday in ATOM format.

If `workday_complete = false`, the date is indicated at the time of report generation. ||
|| **workday_complete**
[`boolean`](../../data-types.md) | Workday completed ||
|| **workday_time_leaks_user**
[`integer`](../../data-types.md) | Duration of the break in seconds ||
|| **workday_time_leaks_final**
[`integer`](../../data-types.md) | Duration of time in seconds that the user underworked or overworked. 
- Positive number — amount of underworked time in seconds
- Negative number — amount of time worked beyond the required time, i.e., overtime. ||
|| **workday_duration**
[`integer`](../../data-types.md) | Duration of the workday according to the schedule in seconds, including breaks ||
|| **workday_duration_final**
[`integer`](../../data-types.md) | Duration of the workday according to actual output in seconds. Includes:
- breaks
- unconfirmed absences
- personal absences ||
|| **workday_duration_config**
[`integer`](../../data-types.md) | Required duration of the workday in seconds ||
|| **reports**
[`array`](../../data-types.md) | List of objects with records of [identified absences](#reports). 

Values are displayed in full detail of the report and for the manager. ||
|| **workday_time_leaks_real**
[`integer`](../../data-types.md) | Duration of the break established by the automatic recording system. Contains unconfirmed absences and personal absences. ||
|#

#### Objects reports {#reports}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../data-types.md) | Record identifier ||
|| **user_id**
[`string`](../../data-types.md) | User identifier ||
|| **type**
[`string`](../../data-types.md) | Record type. Possible values:

- `IDLE` — stepped away, recorded using the desktop application
- `OFFLINE` — offline
- `DESKTOP_ONLINE` — launched the application. Only for the manager
- `DESKTOP_OFFLINE` — turned off the application. Only for the manager
- `DESKTOP_START` — launched the application. Only for the manager
- `TM_START` — started the workday
- `TM_PAUSE` — went on break
- `TM_CONTINUE` — continued the day
- `TM_END` — finished the workday ||
|| **date_start**
[`datetime`](../../data-types.md) | Start date of the recording in ATOM format ||
|| **date_finish**
[`datetime`](../../data-types.md) | End date of the recording in ATOM format.

If `active = true`, the field contains the date at the time of report generation. ||
|| **duration**
[`integer`](../../data-types.md) | Duration ||
|| **active**
[`boolean`](../../data-types.md) | Activity of the record ||
|| **entry_id**
[`string`](../../data-types.md) | Time record identifier ||
|| **report_type**
[`string`](../../data-types.md) | Absence type. Possible values:
- `work` — for work-related issues
- `private` — for personal matters
- `none` — type not specified, equated to `private` ||
|| **report_text**
[`string`](../../data-types.md) | Description of the reason for absence ||
|| **system_text**
[`string`](../../data-types.md) | System description of the reason for absence. Only for the manager ||
|| **source_start**
[`string`](../../data-types.md) | Data source for the start of the record. Possible values:

- `ONLINE_EVENT` — user authorization event
- `OFFLINE_AGENT` — agent that sets the Offline status
- `DESKTOP_OFFLINE_AGENT` — agent that sets the indicator of the application being turned off
- `DESKTOP_ONLINE_EVENT` — event that sets the indicator of the application being turned on
- `DESKTOP_START_EVENT` — event that sets the indicator of the application being turned on
- `IDLE_EVENT` — event changing the status to Stepped Away. Recorded by the application
- `TM_EVENT` — event changing the workday: start, break, end ||
|| **source_finish**
[`string`](../../data-types.md) | Data source for the end of the record. Possible values:

- `ONLINE_EVENT` — user authorization event
- `OFFLINE_AGENT` — agent that sets the Offline status
- `DESKTOP_OFFLINE_AGENT` — agent that sets the indicator of the application being turned off
- `DESKTOP_ONLINE_EVENT` — event that sets the indicator of the application being turned on
- `DESKTOP_START_EVENT` — event that sets the indicator of the application being turned on
- `IDLE_EVENT` — event changing the status to Stepped Away. Recorded by the application
- `TM_EVENT` — event changing the workday: start, break, end ||
|| **ip_start**
[`string`](../../data-types.md) | IP address at the start of the record. Only for the manager ||
|| **ip_finish**
[`string`](../../data-types.md) | IP address at the end of the record. Only for the manager ||
|| **ip_start_network**
[`boolean`](../../data-types.md) \| [`object`](../../data-types.md) | Object with [IP address decoding](#ip_network) for the start of the record, if the IP address is not within the office network. For the office network, it will return `false`.

Only for the manager ||
|| **ip_finish_network**
[`boolean`](../../data-types.md) \| [`object`](../../data-types.md) | Object with [IP address decoding](#ip_network) for the end of the record, if the IP address is not within the office network. For the office network, it will return `false`.

Only for the manager ||
|#

#### Object ip_network {#ip_network}

#|
|| **Name**
`type` | **Description** ||
|| **ip**
[`string`](../../data-types.md) | IP address ||
|| **range**
[`string`](../../data-types.md) | Range that includes the specified IP address ||
|| **name**
[`string`](../../data-types.md) | Name of the range that includes the specified IP address ||
|#

#### Object user {#user}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | User identifier ||
|| **active**
[`boolean`](../../data-types.md) | Activity ||
|| **name**
[`string`](../../data-types.md) | User's full name ||
|| **first_name**
[`string`](../../data-types.md) | User's first name ||
|| **last_name**
[`string`](../../data-types.md) | User's last name ||
|| **work_position**
[`string`](../../data-types.md) | Position ||
|| **avatar**
[`string`](../../data-types.md) | URL of the user's avatar.

If the value is empty, the user has no avatar. ||
|| **personal_gender**
[`string`](../../data-types.md) | User's gender ||
|| **last_activity_date**
[`datetime`](../../data-types.md) | Date of the user's last activity in ATOM format ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "USER_ACCESS_ERROR",
    "error_description": "You don't have access to report for this user"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ACCESS_ERROR` | You don't have access to this method | You do not have access to this method ||
|| `USER_ACCESS_ERROR` | You don't have access to report for this user | You do not have access to this user's reports ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./timeman-timecontrol-report-add.md)
- [{#T}](./timeman-timecontrol-reports-users-get.md)