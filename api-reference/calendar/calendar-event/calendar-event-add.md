# Add Event calendar.event.add

> Scope: [`calendar`](../../scopes/permissions.md)
>
> Who can execute the method: any user

This method adds a new event to the calendar.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **type***
[`string`](../../data-types.md) | Calendar type: 
- `user` — user calendar
- `group` — group calendar
- `company_calendar` — company calendar 
 ||
|| **ownerId***
[`integer`](../../data-types.md) | Identifier of the calendar owner. 

For the company calendar, the `ownerId` parameter has an empty value `""`||
|| **from***
[`datetime`\|`date`](../../data-types.md) | Start date and time of the event.

You can specify a date without time. To do this, pass the value `Y` in the `skip_time` parameter ||
|| **to***
[`datetime`\|`date`](../../data-types.md) | End date of the event.

You can specify a date without time. To do this, pass the value `Y` in the `skip_time` parameter ||
|| **from_ts**
[`integer`](../../data-types.md) | Date and time in timestamp format. Can be used instead of the `from` parameter ||
|| **to_ts**
[`integer`](../../data-types.md) | Date and time in timestamp format. Can be used instead of the `to` parameter ||
|| **section***
[`integer`](../../data-types.md) | Calendar identifier ||
|| **name***
[`string`](../../data-types.md) | Event name ||
|| **skip_time**
[`string`](../../data-types.md) | Pass the date value without time in the `from` and `to` parameters. Possible values:
- `Y` — use only the date
- `N` — use date and time

Date format according to ISO-8601 ||
|| **timezone_from**
[`string`](../../data-types.md) | Timezone of the event's start date and time. Default is the current user's timezone.

The value should be passed as a string, for example, `Europe/Riga` ||
|| **timezone_to**
[`string`](../../data-types.md) | Timezone of the event's end date and time. Default value is the current user's timezone.

The value should be passed as a string, for example, `Europe/Riga` ||
|| **description**
[`text`](../../data-types.md) | Event description ||
|| **color**
[`string`](../../data-types.md) | Background color of the event.

The `#` symbol in the color must be passed in unicode format — `%23` ||
|| **text_color**
[`string`](../../data-types.md) | Text color of the event.

The `#` symbol in the color must be passed in unicode format — `%23` ||
|| **accessibility**
[`string`](../../data-types.md) | Availability during the event time: 
- `busy` — busy 
- `absent` — absent 
- `quest` — tentative 
- `free` — free 
||
|| **importance**
[`string`](../../data-types.md) | Event importance: 
- `high` — high 
- `normal` — medium 
- `low` — low ||
|| **private_event**
[`string`](../../data-types.md) | Mark indicating that the event is private. Possible values:
- `Y` — private
- `N` — not private
||
|| **rrule**
[`object`](../../data-types.md) | Recurrence of the event in the form of an object in terms of the iCalendar standard. Structure described [below](#rrule)
||
|| **is_meeting**
[`string`](../../data-types.md) | Indicator of a meeting with event participants. Possible values:

- `Y` — meeting with participants
- `N` — meeting without participants

For a meeting with participants, specify the list of participants in `attendees` and the event organizer in `host`
||
|| **location**
[`string`](../../data-types.md) | Venue ||
|| **remind**
[`array`](../../data-types.md) | Array of objects describing reminders for the event. Structure described [below](#remind) ||
|| **attendees**
[`array`](../../data-types.md) | List of participant identifiers for the event. If `is_meeting` = `Y` ||
|| **host**
[`integer`](../../data-types.md) | Identifier of the event organizer. If `is_meeting` = `Y` ||
|| **meeting**
[`object`](../../data-types.md) | Object with meeting parameters. Structure described [below](#meeting) ||
|| **crm_fields**
[`array`](../../data-types.md) | Array of CRM entity identifiers to link to the event. Prefixes:
- `CO_` — company
- `C_` — contact 
- `L_` — lead
- `D_` — deal ||
|#

### rrule Parameter {#rrule}

#|
|| **Name**
`type` | **Description** ||
|| **FREQ**
[`string`](../../data-types.md) | Recurrence frequency
- `DAILY` — daily
- `WEEKLY` — weekly
- `MONTHLY` — monthly
- `YEARLY` — yearly
||
|| **COUNT**
[`integer`](../../data-types.md) | Number of recurrences ||
|| **INTERVAL**
[`integer`](../../data-types.md) | Interval between recurrences ||
||**BYDAY**
[`array`](../../data-types.md) | Days of the week
- `SU` — Sunday
- `MO` — Monday
- `TU` — Tuesday
- `WE` — Wednesday
- `TH` — Thursday
- `FR` — Friday
- `SA` — Saturday ||
|| **UNTIL**
[`date`](../../data-types.md) | End date of recurrences ||
|#

### remind Parameter {#remind}

#|
|| **Name**
`type` | **Description** ||
|| **type**
[`string`](../../data-types.md) | Time type of the reminder
- `min` — minutes
- `hour` – hours
- `day` — days ||
|| **count**
[`integer`](../../data-types.md) | Numerical value of the time interval ||
|#

### meeting Parameter {#meeting}

#|
|| **Name**
`type` | **Description** ||
|| **notify**
[`boolean`](../../data-types.md) | Flag for notification of confirmation or refusal by participants ||
|| **reinvite**
[`boolean`](../../data-types.md) | Flag for requesting re-confirmation of participation when editing the event ||
|| **allow_invite**
[`boolean`](../../data-types.md) | Flag for allowing participants to invite others to the event ||
|| **hide_guests**
[`boolean`](../../data-types.md) | Flag for hiding the list of participants ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"type":"user","ownerId":2,"name":"New Event Name","description":"Description for event","from":"2024-06-14","to":"2024-06-14","skip_time":"Y","section":5,"color":"#9cbe1c","text_color":"#283033","accessibility":"absent","importance":"normal","is_meeting":"Y","private_event":"N","remind":[{"type":"min","count":20}],"location":"London","attendees":[1,2,3],"host":2,"meeting":{"notify":true,"reinvite":false,"allow_invite":false,"hide_guests":false},"rrule":{"FREQ":"WEEKLY","BYDAY":["MO","WE"],"COUNT":10,"INTERVAL":1},"crm_fields":["C_5","L_11"]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/calendar.event.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"type":"user","ownerId":2,"name":"New Event Name","description":"Description for event","from":"2024-06-14","to":"2024-06-14","skip_time":"Y","section":5,"color":"#9cbe1c","text_color":"#283033","accessibility":"absent","importance":"normal","is_meeting":"Y","private_event":"N","remind":[{"type":"min","count":20}],"location":"London","attendees":[1,2,3],"host":2,"meeting":{"notify":true,"reinvite":false,"allow_invite":false,"hide_guests":false},"rrule":{"FREQ":"WEEKLY","BYDAY":["MO","WE"],"COUNT":10,"INTERVAL":1},"crm_fields":["C_5","L_11"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/calendar.event.add
    ```

- JS

    ```js
    BX24.callMethod(
        'calendar.event.add',
        {
            type: 'user',
            ownerId: 2,
            name: 'New Event Name',
            description: 'Description for event',
            from: '2024-06-14',
            to: '2024-06-14',
            skip_time: 'Y',
            section: 5,
            color: '#9cbe1c',
            text_color: '#283033',
            accessibility: 'absent',
            importance: 'normal',
            is_meeting: 'Y',
            private_event: 'N',
            remind: [
                {
                    type: 'min',
                    count: 20
                }
            ],
            location: 'London',
            attendees: [1, 2, 3],
            host: 2,
            meeting: {
                notify: true,
                reinvite: false,
                allow_invite: false,
                hide_guests: false,
            },
            rrule: {
                FREQ: 'WEEKLY',
                BYDAY: ['MO', 'WE'],
                COUNT: 10,
                INTERVAL: 1,
            },
            crm_fields: ['C_5', 'L_11']
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'calendar.event.add',
        [
            'type' => 'user',
            'ownerId' => 2,
            'name' => 'New Event Name',
            'description' => 'Description for event',
            'from' => '2024-06-14',
            'to' => '2024-06-14',
            'skip_time' => 'Y',
            'section' => 5,
            'color' => '#9cbe1c',
            'text_color' => '#283033',
            'accessibility' => 'absent',
            'importance' => 'normal',
            'is_meeting' => 'Y',
            'private_event' => 'N',
            'remind' => [
                [
                    'type' => 'min',
                    'count' => 20
                ]
            ],
            'location' => 'London',
            'attendees' => [1, 2, 3],
            'host' => 2,
            'meeting' => [
                'notify' => true,
                'reinvite' => false,
                'allow_invite' => false,
                'hide_guests' => false,
            ],
            'rrule' => [
                'FREQ' => 'WEEKLY',
                'BYDAY' => ['MO', 'WE'],
                'COUNT' => 10,
                'INTERVAL' => 1,
            ],
            'crm_fields' => ['C_5', 'L_11']
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
    "result": 1246,
    "time": {
        "start": 1733318565.183275,
        "finish": 1733318565.695058,
        "duration": 0.5117831230163574,
        "processing": 0.29406094551086426,
        "date_start": "2024-12-04T13:22:45+00:00",
        "date_finish": "2024-12-04T13:22:45+00:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../../data-types.md) | Identifier of the created event ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "The required parameter "name" for the method "calendar.event.add" is not set"
}
```
{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Error Message** | **Description** ||
|| Empty string | The required parameter "type" for the method "calendar.event.add" is not set | The required parameter `type` was not provided ||
|| Empty string | The required parameter "ownerId" for the method "calendar.event.add" is not set | The required parameter `ownerId` was not provided ||
|| Empty string | The required parameter "name" for the method "calendar.event.add" is not set | The required parameter `name` was not provided ||
|| Empty string | The required parameter "from" for the method "calendar.event.add" is not set | The required parameter `from` or `from_ts` was not provided ||
|| Empty string | The required parameter "to" for the method "calendar.event.add" is not set | The required parameter `to` or `to_ts` was not provided ||
|| Empty string | Invalid value for the parameter "name" | Incorrect data format in the `name` field ||
|| Empty string | Invalid value for the parameter "description" | Incorrect data format in the `description` field ||
|| Empty string | Access denied | Creating events in the specified calendar is prohibited ||
|| Empty string | You specified an invalid calendar section ID or the user does not have access to it | An identifier of an inaccessible or non-existent calendar was provided ||
|| Empty string | The list of event links to CRM must be an array | Incorrect data format in the `crm_fields` field ||
|| Empty string | An error occurred while creating the event | Another error ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}