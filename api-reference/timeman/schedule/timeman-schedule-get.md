# Get Work Schedule timeman.schedule.get

> Scope: [`timeman`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `timeman.schedule.get` retrieves the work schedule by its identifier. If no schedule with the specified identifier exists, it will return an empty array.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Identifier of the schedule.

You can find the schedule identifier in the list of schedules on the *Employees > Time and Reports > Work Schedules* page ||
|#

## Code Examples

{% include [Examples Note](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/timeman.schedule.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/timeman.schedule.get
    ```

- JS

    ```js
    BX24.callMethod(
        "timeman.schedule.get",
        {
            id: 1
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
            {
                console.dir(result.data());
                if(result.more())
                    result.next();
            }
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'timeman.schedule.get',
        [
            'id' => 1
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
        "ID": 1,
        "NAME": "For all employees",
        "SCHEDULE_TYPE": "FIXED",
        "REPORT_PERIOD": "MONTH",
        "REPORT_PERIOD_OPTIONS": {
            "START_WEEK_DAY": 0
        },
        "CALENDAR_ID": 1,
        "ALLOWED_DEVICES": {
            "browser": true
        },
        "DELETED": "0",
        "IS_FOR_ALL_USERS": true,
        "WORKTIME_RESTRICTIONS": [
            "[]"
        ],
        "CONTROLLED_ACTIONS": 3,
        "UPDATED_BY": 503,
        "DELETED_BY": 0,
        "DELETED_AT": "",
        "CREATED_BY": 0,
        "CREATED_AT": "2019-09-19T21:22:22+02:00",
        "SHIFTS": [
            {
                "ID": 1,
                "NAME": "",
                "BREAK_DURATION": 3600,
                "WORK_TIME_START": 28800,
                "WORK_TIME_END": 61200,
                "WORK_DAYS": "12345",
                "SCHEDULE_ID": 1,
                "DELETED": false
            }
        ],
        "CALENDAR": {
            "ID": 1,
            "NAME": "",
            "PARENT_CALENDAR_ID": 0,
            "SYSTEM_CODE": "",
            "EXCLUSIONS": []
        },
        "SCHEDULE_VIOLATION_RULES": {
            "ID": 1,
            "SCHEDULE_ID": 1,
            "ENTITY_CODE": "UA",
            "MAX_EXACT_START": 28859,
            "MIN_EXACT_END": 61200,
            "MAX_OFFSET_START": -1,
            "MIN_OFFSET_END": -1,
            "RELATIVE_START_FROM": -1,
            "RELATIVE_START_TO": -1,
            "RELATIVE_END_FROM": -1,
            "RELATIVE_END_TO": -1,
            "MIN_DAY_DURATION": 28800,
            "MAX_ALLOWED_TO_EDIT_WORK_TIME": 300,
            "MAX_WORK_TIME_LACK_FOR_PERIOD": 3600,
            "PERIOD_TIME_LACK_AGENT_ID": 309429,
            "MAX_SHIFT_START_DELAY": -1,
            "MISSED_SHIFT_START": 0,
            "USERS_TO_NOTIFY": {
                "FIXED_START_END": [
                    "U503"
                ],
                "FIXED_PER_RECORD": [
                    "U503"
                ],
                "FIXED_EDIT_WORKTIME": [
                    "U503"
                ],
                "FIXED_PERIODIC": [
                    "U503"
                ],
                "SHIFT_DELAY": [],
                "SHIFT_MISSED_START": []
            }
        }
    },
    "time": {
        "start": 1744036659.2339499,
        "finish": 1744036659.2655749,
        "duration": 0.031625032424926758,
        "processing": 0.008758068084716797,
        "date_start": "2025-04-07T17:37:39+02:00",
        "date_finish": "2025-04-07T17:37:39+02:00",
        "operating_reset_at": 1744037259,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response.

Contains an object describing the work schedule of employees ||
|| **ID**
[`integer`](../../data-types.md) | Identifier of the work schedule ||
|| **NAME**
[`string`](../../data-types.md) | Name of the work schedule || 
|| **SCHEDULE_TYPE**
[`string`](../../data-types.md) | Type of work schedule.

Possible values:
- `FIXED` — fixed
- `SHIFT` — shift-based
- `FLEXTIME` — flexible ||
|| **REPORT_PERIOD**
[`string`](../../data-types.md) | Frequency of report generation for the schedule.

Possible values:
- `MONTH` — month
- `WEEK` — week
- `TWO_WEEKS` — two weeks
- `QUARTER`  — quarter ||
|| **REPORT_PERIOD_OPTIONS**
[`object`](../../data-types.md) | Object describing [additional settings](#report_period_options) for report generation period.

Only for `REPORT_PERIOD` with values `WEEK` and `TWO_WEEKS` ||
|| **CALENDAR_ID**
[`integer`](../../data-types.md) | Identifier of the calendar associated with the work schedule ||
|| **ALLOWED_DEVICES**
[`object`](../../data-types.md) | Object describing [allowed devices](#allowed_devices) for time tracking ||
|| **DELETED**
[`string`](../../data-types.md) | Flag indicating if the work schedule is deleted.

Value `"0"` means the schedule is active. Value `"1"` indicates a deleted state ||
|| **IS_FOR_ALL_USERS**
[`boolean`](../../data-types.md) | Applicability of the schedule to all employees.

Value `true` means the schedule applies to all users of the system ||
|| **WORKTIME_RESTRICTIONS**
[`array`](../../data-types.md) | Work time restrictions.

Contains an array of strings with restriction rules ||
|| **CONTROLLED_ACTIONS**
[`integer`](../../data-types.md) | Number of controlled actions within the work schedule ||
|| **UPDATED_BY**
[`integer`](../../data-types.md) | Identifier of the user who last updated the work schedule ||
|| **DELETED_BY**
[`integer`](../../data-types.md) | Identifier of the user who deleted the work schedule.

Value `0` indicates that the schedule was not deleted ||
|| **DELETED_AT**
[`string`](../../data-types.md) | Date and time of deletion of the work schedule.

An empty string means the schedule was not deleted ||
|| **CREATED_BY**
[`integer`](../../data-types.md) | Identifier of the user who created the work schedule.

Value `0` indicates system creation ||
|| **CREATED_AT**
[`datetime`](../../data-types.md) | Date and time of creation of the work schedule ||
|| **SHIFTS**
[`array`](../../data-types.md) | Array of shift objects. Each object contains a description of the [shift](#shifts) associated with the work schedule ||
|| **CALENDAR**
[`object`](../../data-types.md) | Object with information about the [calendar](#calendar) associated with the work schedule ||
|| **SCHEDULE_VIOLATION_RULES**
[`object`](../../data-types.md) | Object describing [schedule violation rules](#schedule_violation_rules) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

#### Object REPORT_PERIOD_OPTIONS {#report_period_options}

#|
|| **Name**
`type` | **Description** ||
|| **START_WEEK_DAY**
[`integer`](../../data-types.md) | Day the week starts.

Possible values:
- `0` — Monday
- `1` — Tuesday
- `2` — Wednesday
- `3` — Thursday
- `4` — Friday
- `5` — Saturday 
- `6` — Sunday ||
|#

#### Object ALLOWED_DEVICES {#allowed_devices}

#|
|| **Name**
`type` | **Description** ||
|| **browser**
[`boolean`](../../data-types.md) | Is time tracking allowed through the browser.

If `true` — tracking is allowed ||
|#

#### Object SHIFTS {#shifts}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../../data-types.md) | Identifier of the shift ||
|| **NAME**
[`string`](../../data-types.md) | Name of the shift ||
|| **BREAK_DURATION**
[`integer`](../../data-types.md) | Duration of the break in seconds ||
|| **WORK_TIME_START**
[`integer`](../../data-types.md) | Start time of the workday in seconds from midnight ||
|| **WORK_TIME_END**
[`integer`](../../data-types.md) | End time of the workday in seconds from midnight ||
|| **WORK_DAYS**
[`string`](../../data-types.md) | String with codes of workdays. For example, `01234` — from Monday to Friday ||
|| **SCHEDULE_ID**
[`integer`](../../data-types.md) | Identifier of the work schedule ||
|| **DELETED**
[`boolean`](../../data-types.md) | Flag indicating if the shift is deleted.

Value `true` means the shift is deleted ||
|#

#### Object CALENDAR {#calendar}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../../data-types.md) | Identifier of the calendar ||
|| **NAME**
[`string`](../../data-types.md) | Name of the calendar ||
|| **PARENT_CALENDAR_ID**
[`integer`](../../data-types.md) | Identifier of the parent calendar.

Value `0` indicates that there is no parent calendar ||
|| **SYSTEM_CODE**
[`string`](../../data-types.md) | System code of the calendar ||
|| **EXCLUSIONS**
[`array`](../../data-types.md) | Exclusions from the calendar.

Contains an array of strings with dates or periods excluded from the calendar ||
|#

#### Object SCHEDULE_VIOLATION_RULES {#schedule_violation_rules}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../../data-types.md) | Identifier of the schedule violation rules ||
|| **SCHEDULE_ID**
[`integer`](../../data-types.md) | Identifier of the work schedule ||
|| **ENTITY_CODE**
[`string`](../../data-types.md) | Code of the entity to which the rules apply.

For example, `UA` — user actions ||
|| **MAX_EXACT_START**
[`integer`](../../data-types.md) | Maximum exact start time of the workday in seconds from midnight ||
|| **MIN_EXACT_END**
[`integer`](../../data-types.md) | Minimum exact end time of the workday in seconds from midnight ||
|| **MAX_OFFSET_START**
[`integer`](../../data-types.md) | Maximum offset for the start of the workday.

Value `-1` means no restriction is set ||
|| **MIN_OFFSET_END**
[`integer`](../../data-types.md) | Minimum offset for the end of the workday.

Value `-1` means no restriction is set ||
|| **RELATIVE_START_FROM**
[`integer`](../../data-types.md) | Relative start of the workday (relative to planned time).

Value `-1` means no restriction is set ||
|| **RELATIVE_START_TO**
[`integer`](../../data-types.md) | Relative end of the workday (relative to planned time).

Value `-1` means no restriction is set ||
|| **RELATIVE_END_FROM**
[`integer`](../../data-types.md) | Relative start of the end of the workday.

Value `-1` means no restriction is set ||
|| **RELATIVE_END_TO**
[`integer`](../../data-types.md) | Relative end of the workday.

Value `-1` means no restriction is set ||
|| **MIN_DAY_DURATION**
[`integer`](../../data-types.md) | Minimum duration of the workday in seconds ||
|| **MAX_ALLOWED_TO_EDIT_WORK_TIME**
[`integer`](../../data-types.md) | Maximum time allowed to edit work time in seconds ||
|| **MAX_WORK_TIME_LACK_FOR_PERIOD**
[`integer`](../../data-types.md) | Maximum time deficit for the period in seconds ||
|| **PERIOD_TIME_LACK_AGENT_ID**
[`integer`](../../data-types.md) | Identifier of the agent checking the deficit for the period ||
|| **MAX_SHIFT_START_DELAY**
[`integer`](../../data-types.md) | Maximum delay for the start of the shift in seconds.

Value `-1` means no restriction is set ||
|| **MISSED_SHIFT_START**
[`integer`](../../data-types.md) | Flag indicating if the start of the shift was missed.

Value `0` means no missed start was recorded ||
|| **USERS_TO_NOTIFY**
[`object`](../../data-types.md) | Object describing users for [notification of schedule violations](#users_to_notify) ||
|#

#### Object USERS_TO_NOTIFY {#users_to_notify}

#|
|| **Name**
`type` | **Description** ||
|| **FIXED_START_END**
[`array`](../../data-types.md) | List of users to notify about fixed start and end of the workday.

Each element of the array contains the user identifier in the format `U<ID>` ||
|| **FIXED_PER_RECORD**
[`array`](../../data-types.md) | List of users to notify about fixed time records.

Each element of the array contains the user identifier in the format `U<ID>` ||
|| **FIXED_EDIT_WORKTIME**
[`array`](../../data-types.md) | List of users to notify about changes in work time.

Each element of the array contains the user identifier in the format `U<ID>` ||
|| **FIXED_PERIODIC**
[`array`](../../data-types.md) | List of users to notify about periodic schedule violations.

Each element of the array contains the user identifier in the format `U<ID>` ||
|| **SHIFT_DELAY**
[`array`](../../data-types.md) | List of users to notify about delays in the start of the shift ||
|| **SHIFT_MISSED_START**
[`array`](../../data-types.md) | List of users to notify about missed starts of the shift ||
|#

## Error Handling

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)