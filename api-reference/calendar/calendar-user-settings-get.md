# Get User Calendar Settings calendar.user.settings.get

> Scope: [`calendar`](../scopes/permissions.md)
>
> Who can execute the method: any user

This method retrieves user calendar settings.

No parameters.

## Code Examples

{% include [Examples Note](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/calendar.user.settings.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/calendar.user.settings.get
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'calendar.user.settings.get',
    		{}
    	);
    	
    	const result = response.getData().result;
    	console.log('Result:', result);
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
                'calendar.user.settings.get',
                []
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your logic for processing data
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting user calendar settings: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'calendar.user.settings.get',
        {}
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'calendar.user.settings.get',
        []
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
    "result": {
        "view": "month",
        "meetSection": "4",
        "crmSection": "4",
        "showDeclined": true,
        "denyBusyInvitation": false,
        "collapseOffHours": "N",
        "showWeekNumbers": "N",
        "showTasks": "Y",
        "syncTasks": "N",
        "showCompletedTasks": "N",
        "lastUsedSection": "false",
        "sendFromEmail": "",
        "defaultSections": {
            "user1": "4",
            "group6": "49"
        },
        "syncPeriodPast": "3",
        "syncPeriodFuture": "12",
        "defaultReminders": {
            "fullDay": [
                {
                    "type": "min",
                    "count": 15
                }
            ],
            "withTime": [
                {
                    "type": "min",
                    "count": 50
                }
            ]
        },
        "timezoneName": "Europe/Berlin",
        "timezoneOffsetUTC": 7200,
        "timezoneDefaultName": "",
        "work_time_start": "9.00",
        "work_time_end": "19.00"
    },
    "time": {
        "start": 1734434829.455204,
        "finish": 1734434829.797482,
        "duration": 0.34227800369262695,
        "processing": 0.0038161277770996094,
        "date_start": "2024-12-17T11:27:09+00:00",
        "date_finish": "2024-12-17T11:27:09+00:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | Root element of the response ||
|| **view**
[`string`](../data-types.md) | Standard view for the calendar. Possible values:
- `day` — day
- `week` — week
- `month` — month
- `list` — list ||
|| **meetSection**
[`string`](../data-types.md) | Calendar for invitations ||
|| **crmSection**
[`string`](../data-types.md) | Calendar for CRM ||
|| **showDeclined**
[`boolean`](../data-types.md) | Show events where the user declined to participate ||
|| **denyBusyInvitation**
[`boolean`](../data-types.md) | Prevent inviting to an event if the time is busy ||
|| **collapseOffHours**
[`string`](../data-types.md) | Hide non-working hours in the calendar in weekly and daily views. Possible values:
- `Y` — hide
- `N` — do not hide ||
|| **showWeekNumbers**
[`string`](../data-types.md) | Show week numbers. Possible values:
- `Y` — show
- `N` — do not show ||
|| **showTasks**
[`string`](../data-types.md) | Display tasks in the calendar. Possible values:
- `Y` — display
- `N` — do not display ||
|| **syncTasks**
[`string`](../data-types.md) | Synchronize task calendar. Possible values:
- `Y` — yes
- `N` — no ||
|| **showCompletedTasks**
[`string`](../data-types.md) | Display completed tasks. Possible values:
- `Y` — display
- `N` — do not display  ||
|| **lastUsedSection**
[`string`](../data-types.md) | Identifier of the calendar used when creating events if the calendar identifier is not provided in the parameters. 

Default value — `false` ||
|| **sendFromEmail**
[`string`](../data-types.md) | E-mail for sending mail invitations ||
|| **defaultSections**
[`object`](../data-types.md) | Settings for preset calendars.

The key of the settings object can be:
- `user[id]` — type User calendar with user identifier. For example, `user12` corresponds to the user calendar with identifier `12`
- `group[id]` — type Group calendar with group identifier. For example, `group36` corresponds to the group calendar with identifier `36`

The value of the object is the calendar identifier ||
|| **syncPeriodPast**
[`string`](../data-types.md) | Number of months for synchronization in the past period ||
|| **syncPeriodFuture**
[`string`](../data-types.md) | Number of months for synchronization in the future period ||
|| **defaultReminders**
[`object`](../data-types.md) | Object with standard settings for [event reminders](#defaultReminders) ||
|| **timezoneName**
[`string`](../data-types.md) | Calendar timezone ||
|| **timezoneOffsetUTC**
[`integer`](../data-types.md) | Timezone offset relative to UTC in seconds ||
|| **timezoneDefaultName**
[`string`](../data-types.md) | If the `timezoneName` parameter is not set, the timezone from the `timezoneOffsetUTC` parameter will be specified here ||
|| **work_time_start**
[`string`](../data-types.md) | Start time of the workday ||
|| **work_time_end**
[`string`](../data-types.md) | End time of the workday ||
|#

#### Object defaultReminders {#defaultReminders}

#|
|| **Name**
`type` | **Description** ||
|| **fullDay**
[`array`](../data-types.md) | Array of [standard reminder settings](#reminder-settings) for all-day events ||
|| **withTime**
[`array`](../data-types.md) | Array of [standard reminder settings](#reminder-settings) for events with specified time ||
|#

#### Reminder Settings Object {#reminder-settings}

#|
|| **Name**
`type` | **Description** ||
|| **type**
[`string`](../data-types.md) | Time type of the reminder. Possible values:
- `min` — minutes
- `hour` — hours
- `day` — days ||
|| **count**
[`integer`](../data-types.md) | Numeric value of the time interval ||
|#

## Error Handling

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./calendar-settings-get.md)
- [{#T}](./calendar-user-settings-set.md)