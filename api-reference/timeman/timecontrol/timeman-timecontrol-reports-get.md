# Get Report on Identified Absences timeman.timecontrol.reports.get

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types not specified
- examples are missing

{% endnote %}

{% endif %}

> Scope: [`timeman`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `timeman.timecontrol.reports.get` is used to retrieve a report on identified absences.

### Parameters

#|
|| **Parameter** | **Example** | **Required** | **Description** ||
|| **USER_ID**^*^
[`unknown`](../../data-types.md) | 2 | Yes | User identifier. ||
|| **YEAR**^*^
[`unknown`](../../data-types.md) | 2018 | Yes | Year for report generation. ||
|| **MONTH**^*^
[`unknown`](../../data-types.md) | 8 | Yes | Month for report generation. ||
|| **WORKDAY_HOURS**^**^
[`unknown`](../../data-types.md) | 8 | No | Required duration of the workday in hours (default is 8 hours). ||
|| **IDLE_MINUTES**
[`unknown`](../../data-types.md) | 15 | No | Maximum time of absence at the workplace that will not be counted as absence. ||
|#

{% include [Footnote on parameters](../../../_includes/required.md) %}

^**^ – The parameter `IDLE_MINUTES` is available only if you are a manager or administrator. If not specified, the time is automatically taken from the module settings.

## Example Call

{% list tabs %}

- JS

    ```js
    BX24.callMethod('timeman.timecontrol.reports.get', {
        user_id: 2,
        year: 2018,
        month: 8,
        workday_hours: 8,
        idle_minutes: 15
    }, function(result){
        if(result.error())
        {
            console.error(result.error().ex);
        }
        else
        {
            console.log(result.data());
        }
    });
    ```

- PHP

    ```php
    $result = restCommand('timeman.timecontrol.reports.get', Array(
        'USER_ID' => 2,
        'YEAR' => 2018,
        'MONTH' => 8,
        'WORKDAY_HOURS' => 8,
        'IDLE_MINUTES' => 15
    ), $_REQUEST["auth"]);
    ```

{% endlist %}

{% include [Footnote on examples](../../../_includes/examples.md) %}

## Successful Response

> 200 OK
```json
{
    "result": {
        "report": {
            "month_title": "August",
            "date_start": "2018-08-01T00:00:00+02:00",
            "date_finish": "2018-08-31T23:59:59+02:00",
            "days": [
                {
                    "index": "20180816",
                    "day_title": "08/16/2018",
                    "workday_complete": false,
                    "workday_date_start": "2018-08-16T14:08:35+02:00",
                    "workday_date_finish": "2018-08-16T11:20:00+02:00",
                    "workday_duration": 10115,
                    "workday_duration_config": 28800,
                    "workday_duration_final": 6914,
                    "workday_time_leaks_user": 0,
                    "workday_time_leaks_real": 3201,
                    "workday_time_leaks_final": 21886,
                    "reports": [
                        {
                            "id": 459,
                            "type": "TM_START",
                            "date_start": "2018-08-16T14:08:35+02:00",
                            "date_finish": "2018-08-16T14:08:35+02:00",
                            "duration": 0,
                            "active": false,
                            "entry_id": 35,
                            "report_type": "NONE",
                            "report_text": null,
                            "system_text": "",
                            "source_start": "TM_EVENT",
                            "source_finish": "TM_EVENT",
                            "ip_start": "93.60.295.11",
                            "ip_finish": "10.10.255.255",
                            "ip_start_network": false,
                            "ip_finish_network": {
                                "ip": "10.10.255.255",
                                "range": "10.0.0.0-10.255.255.255",
                                "name": "Office Network 10.x.x.x"
                            }
                        }
                    ]
                }
            ]
        },
        "user": {
            "id": 2,
            "name": "Maria Ivshina",
            "first_name": "Maria",
            "last_name": "Ivshina",
            "work_position": "IT Specialist",
            "avatar": "http://test.bitrix24.com/upload/resize_cache/main/072/100_100_2/42-17948709.gif",
            "last_activity_date": "2018-08-15T16:25:34+02:00"
        }
    }
}
```

### Key Descriptions

- **report** - information about the report.
- **month_title** - name of the month.
- **date_start** - start date of the selection period in ATOM format.
- **date_finish** - end date of the selection period in ATOM format.
- **days** - list of worked days.
    - **index** - index of the day of the week.
    - **day_title** - date (in site format).
    - **workday_complete** - workday completed (true / false).
    - **workday_date_start** - start date of the workday in ATOM format.
    - **workday_date_finish** - end date of the workday in ATOM format (if `workday_complete = false`, this field shows the date at the time of report generation).
    - **workday_duration** - duration of the workday according to the timesheet in seconds (considering the break set by the user).
    - **workday_duration_final** - duration of the workday based on actual work in seconds (considering the break set by the user, unconfirmed absences, and absences for personal matters).
    - **workday_duration_config** - required duration of the workday in seconds.
    - **workday_time_leaks_user** - duration of the break set by the user in seconds.
    - **workday_time_leaks_real** - duration of the break recorded by the automatic tracking system (unconfirmed absences and absences for personal matters).
    - **workday_time_leaks_final** - if the number is positive: amount of unworked time in seconds; if the number is negative: amount of time worked beyond the required time (overtime).
    - **reports** - list of records of identified absences (values are available only at the head and full detail levels).
     - **id** - identifier of the record, necessary for using the method `timeman.timecontrol.report.add`.
     - **type** - type of record (the directory of types is described below).
     - **active** - activity status of the record.
     - **date_start**- start date of the record in ATOM format.
     - **date_finish** - end date of the record in ATOM format (if active = true, this field shows the date at the time of report generation).
     - **report_type** - type of absence (work - for work matters, private - personal matters, none - not specified, equated to private).
     - **report_text** - description of the reason for absence.
     - **system_text** - system description of the reason for absence (parameter available only at the head detail level).
     - **source_start** - data provider for the start of the record (the directory of types is described below).
     - **source_finish** - data provider for the end of the record (the directory of types is described below).
     - **ip_start** - IP address at the start of the record (parameter available only at the head detail level).
     - **ip_start_network** - decoding of the IP address at the start of the record, if the IP address is not in the office network - false (parameter available only at the head detail level).
        - **ip** - IP address at the start of the record.
        - **range** - range that includes the specified IP address.
        - **name** - name of the range that includes the specified IP address.
     - **ip_finish** - IP address at the end of the record (parameter available only at the head detail level).
     - **ip_finish_network** - decoding of the IP address at the end of the record, if the IP address is not in the office network - false (parameter available only at the head detail level).
        - **ip** - IP address at the end of the record.
        - **range** - range that includes the specified IP address.
        - **name** - name of the range that includes the specified IP address.

- **user** - information about the user.
- **id** - user identifier.
- **name** - full name of the user.
- **first_name** - user's first name.
- **last_name** - user's last name.
- **work_position** - position.
- **avatar** - link to the avatar (if empty, it means the avatar is not set).
- **personal_gender** - user's gender.
- **last_activity_date** - date of the user's last action in ATOM format.

#### Possible Record Types (type)

- **IDLE** - Away (recorded using the desktop application).
- **OFFLINE** - Offline.
- **DESKTOP_ONLINE** - Desktop application launch recorded (type available only at the head detail level).
- **DESKTOP_OFFLINE** - Desktop application shutdown recorded (type available only at the head detail level).
- **DESKTOP_START** - Desktop application launch recorded (type available only at the head detail level).
- **TM_START** - Workday started.
- **TM_PAUSE** - On break.
- **TM_CONTINUE** - Continued the day.
- **TM_END** - Workday ended.

#### Possible Record Sources (source_start, source_finish)

- **ONLINE_EVENT** - User authorization event.
- **OFFLINE_AGENT** - Agent marking offline status.
- **DESKTOP_OFFLINE_AGENT** - Agent marking the desktop application as shut down.
- **DESKTOP_ONLINE_EVENT** - Event marking the desktop application as online.
- **DESKTOP_START_EVENT** - Event marking the desktop application as started.
- **IDLE_EVENT** - Event changing the status to away (recorded using the desktop application).
- **TM_EVENT** - Event changing the workday (start, break, end).

## Error Response

> 200 Error, 50x Error
```json
{
    "error": "ACCESS_ERROR",
    "error_description": "You don't have access to this method"
}
```

### Key Descriptions

- Key **error** - code of the occurred error.
- Key **error_description** - brief description of the occurred error.

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| **ACCESS_ERROR** | The specified method is not available to the user. ||
|| **USER_ACCESS_ERROR** | No access to the specified user. ||
|#

## If Returns Empty Days

If an empty array of days is returned, first set the necessary options for accessing the report and collecting data (you need to be an administrator and execute the following in the console on any page of the account):

```js
BX24.callMethod('timeman.timecontrol.settings.set', {
    active: true,
    REPORT_SIMPLE_TYPE: 'all',
    REPORT_FULL_TYPE: 'all',
    report_request_type: 'user',
    report_request_users: [BX.message.USER_ID],
}, function(result){
    if(result.error())
    {
        console.error(result.error().ex);
    }
    else
    {
        console.log(result.data());
    }
});
```

After that, open or close the workday, and then the report for this user will have data:

```js
BX24.callMethod('timeman.timecontrol.reports.get', {
    user_id: BX.message.USER_ID,
    year: 2019,
    month: 1,
    workday_hours: 8,
    idle_minutes: 15
}, function(result){
    if(result.error())
    {
        console.error(result.error().ex);
    }
    else
    {
        console.log(result.data());
    }
});
```