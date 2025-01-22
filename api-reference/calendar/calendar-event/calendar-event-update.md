# Update Event calendar.event.update

> Scope: [`calendar`](../../scopes/permissions.md)
>
> Who can execute the method: any user

This method updates an existing event.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Event identifier.

You can obtain the identifier using the [calendar.event.get](./calendar-event-get.md) or [calendar.event.get.nearest](./calendar-event-get-nearest.md) methods. ||
|| **type***
[`string`](../../data-types.md) | Calendar type: 
- `user` — user calendar
- `group` — group calendar
- `company_calendar` — company calendar  ||
|| **ownerId***
[`integer`](../../data-types.md) | Identifier of the calendar owner. 

For a company calendar, the `ownerId` parameter has an empty value `""` ||
|| **from**
[`datetime`\|`date`](../../data-types.md) | Start date and time of the event.

You can specify a date without time. To do this, pass the value `Y` in the `skip_time` parameter. ||
|| **to**
[`datetime`\|`date`](../../data-types.md) | End date of the event.

You can specify a date without time. To do this, pass the value `Y` in the `skip_time` parameter. ||
|| **from_ts**
[`integer`](../../data-types.md) | Date and time in timestamp format. Can be used instead of the `from` parameter. ||
|| **to_ts**
[`integer`](../../data-types.md) | Date and time in timestamp format. Can be used instead of the `to` parameter. ||
|| **section**
[`integer`](../../data-types.md) | Calendar identifier. ||
|| **name***
[`string`](../../data-types.md) | Event name. ||
|| **skip_time**
[`string`](../../data-types.md) | Pass the date value without time in the `from` and `to` parameters. Possible values:
- `Y` — use only the date
- `N` — use date and time

Date format according to ISO-8601 standard. ||
|| **timezone_from**
[`string`](../../data-types.md) | Timezone of the event's start date and time. Default is the current user's timezone.

The value should be passed as a string, for example, `Europe/Riga`. ||
|| **timezone_to**
[`string`](../../data-types.md) | Timezone of the event's end date and time. Default value is the current user's timezone.

The value should be passed as a string, for example, `Europe/Riga`. ||
|| **description**
[`text`](../../data-types.md) | Event description. ||
|| **color**
[`string`](../../data-types.md) | Background color of the event.

The `#` symbol in the color must be passed in unicode format — `%23`. ||
|| **text_color**
[`string`](../../data-types.md) | Text color of the event.

The `#` symbol in the color must be passed in unicode format — `%23`. ||
|| **accessibility**
[`string`](../../data-types.md) | Availability during the event time: 
- `busy` — busy 
- `absent` — absent 
- `quest` — tentative 
- `free` — free  ||
|| **importance**
[`string`](../../data-types.md) | Importance of the event: 
- `high` — high 
- `normal` — medium 
- `low` — low. ||
|| **private_event**
[`string`](../../data-types.md) | Mark indicating that the event is private. Possible values:
- `Y` — private
- `N` — not private. ||
|| **recurrence_mode**
[`string`](../../data-types.md) | Parameter for partial editing of a recurring event. Possible values:
- `this` — changes apply only to the current event. `current_date_from` must be specified.
- `next` — changes apply to the current and all following events. `current_date_from` must be specified.
- `all` — changes apply to all events in the recurrence chain. ||
|| **current_date_from**
[`date`](../../data-types.md) | Date of the current event for partial editing of a recurring event. ||
|| **rrule**
[`object`](../../data-types.md) | Recurrence of the event as an object in terms of the iCalendar standard. The structure is described [below](#rrule). ||
|| **is_meeting**
[`string`](../../data-types.md) | Indicator of a meeting with event participants. Possible values:

- `Y` — meeting with participants
- `N` — meeting without participants

For a meeting with participants, specify the list of attendees `attendees` and the event organizer `host`. ||
|| **location**
[`string`](../../data-types.md) | Venue. ||
|| **remind**
[`array`](../../data-types.md) | Array of objects describing reminders for the event. The structure is described [below](#remind). ||
|| **attendees**
[`array`](../../data-types.md) | List of identifiers of event participants. If `is_meeting` = `Y`. ||
|| **host**
[`string`](../../data-types.md) | Identifier of the event organizer. If `is_meeting` = `Y`. ||
|| **meeting**
[`object`](../../data-types.md) | Object with meeting parameters. The structure is described [below](#meeting). ||
|| **crm_fields**
[`array`](../../data-types.md) | Array of CRM object identifiers to link to the event. To link objects, list their identifiers with [prefixes](../../crm/data-types.md#object_type):
- `CO_` — company
- `C_` — contact 
- `L_` — lead
- `D_` — deal. ||
|#

### Parameter rrule {#rrule}

#|
|| **Name**
`type` | **Description** ||
|| **FREQ**
[`string`](../../data-types.md) | Recurrence frequency
- `DAILY` — daily
- `WEEKLY` — weekly
- `MONTHLY` — monthly
- `YEARLY` — yearly. ||
|| **COUNT**
[`integer`](../../data-types.md) | Number of recurrences. ||
|| **INTERVAL**
[`integer`](../../data-types.md) | Interval between recurrences. ||
||**BYDAY**
[`array`](../../data-types.md) | Days of the week
- `SU` — Sunday
- `MO` — Monday
- `TU` — Tuesday
- `WE` — Wednesday
- `TH` — Thursday
- `FR` — Friday
- `SA` — Saturday. ||
|| **UNTIL**
[`date`](../../data-types.md) | End date of recurrences. ||
|#

### Parameter remind {#remind}

#|
|| **Name**
`type` | **Description** ||
|| **type**
[`string`](../../data-types.md) | Time type of the reminder
- `min` — minutes
- `hour` – hours
- `day` — days. ||
|| **count**
[`integer`](../../data-types.md) | Numerical value of the time interval. ||
|#

### Parameter meeting {#meeting}

#|
|| **Name**
`type` | **Description** ||
|| **notify**
[`boolean`](../../data-types.md) | Flag for notification of confirmation or refusal by participants. ||
|| **reinvite**
[`boolean`](../../data-types.md) | Flag for requesting re-confirmation of participation when editing the event. ||
|| **allow_invite**
[`boolean`](../../data-types.md) | Flag allowing participants to invite others to the event. ||
|| **hide_guests**
[`boolean`](../../data-types.md) | Flag for hiding the list of participants. ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":699,"type":"user","ownerId":2,"name":"Changed Event Name","description":"New description for event","from":"2024-06-17","to":"2024-06-17","skip_time":"Y","section":5,"color":"#9cbe1c","text_color":"#283033","accessibility":"free","importance":"normal","is_meeting":"Y","private_event":"Y","recurrence_mode":"next","current_date_from":"2024-12-04","remind":[{"type":"min","count":10}],"location":"London","attendees":[1,2,3],"host":2,"meeting":{"notify":true,"reinvite":false,"allow_invite":false,"hide_guests":false},"rrule":{"FREQ":"WEEKLY","BYDAY":["MO","WE"],"COUNT":10,"INTERVAL":1},"crm_fields":["C_5","L_11"]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/calendar.event.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":699,"type":"user","ownerId":2,"name":"Changed Event Name","description":"New description for event","from":"2024-06-17","to":"2024-06-17","skip_time":"Y","section":5,"color":"#9cbe1c","text_color":"#283033","accessibility":"free","importance":"normal","is_meeting":"Y","private_event":"Y","recurrence_mode":"next","current_date_from":"2024-12-04","remind":[{"type":"min","count":10}],"location":"London","attendees":[1,2,3],"host":2,"meeting":{"notify":true,"reinvite":false,"allow_invite":false,"hide_guests":false},"rrule":{"FREQ":"WEEKLY","BYDAY":["MO","WE"],"COUNT":10,"INTERVAL":1},"crm_fields":["C_5","L_11"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/calendar.event.update
    ```

- JS

    ```js
    BX24.callMethod(
        'calendar.event.update',
        {
            id: 699,
            type: 'user',
            ownerId: 2,
            name: 'Changed Event Name',
            description: 'New description for event',
            from: '2024-06-17',
            to: '2024-06-17',
            skip_time: 'Y',
            section: 5,
            color: '#9cbe1c',
            text_color: '#283033',
            accessibility: 'free',
            importance: 'normal',
            is_meeting: 'Y',
            private_event: 'Y',
            recurrence_mode: 'next',
            current_date_from: '2024-12-04',
            remind: [
                {
                    type: 'min',
                    count: 10
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
        'calendar.event.update',
        [
            'id' => 699,
            'type' => 'user',
            'ownerId' => 2,
            'name' => 'Changed Event Name',
            'description' => 'New description for event',
            'from' => '2024-06-17',
            'to' => '2024-06-17',
            'skip_time' => 'Y',
            'section' => 5,
            'color' => '#9cbe1c',
            'text_color' => '#283033',
            'accessibility' => 'free',
            'importance' => 'normal',
            'is_meeting' => 'Y',
            'private_event' => 'Y',
            'recurrence_mode' => 'next',
            'current_date_from' => '2024-12-04',
            'remind' => [
                [
                    'type' => 'min',
                    'count' => 10
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

HTTP status: **200**

{% list tabs %}

- Single Event

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

- Recurring Event

    ```json
    {
        "result": {
            "originalDate": "12/24/2024 05:59:00 pm",
            "originalDavXmlId": "20241205T124346Z-e3ccab8aebc16d0c5cccecb63fef2bc3@b24evo.com",
            "instanceTz": "Europe/Riga",
            "recEventId": 1261,
            "id": 1260
        },
        "time": {
            "start": 1733402626.139122,
            "finish": 1733402626.740063,
            "duration": 0.6009409427642822,
            "processing": 0.3765721321105957,
            "date_start": "2024-12-05T12:43:46+00:00",
            "date_finish": "2024-12-05T12:43:46+00:00"
        }
    }
    ```

{% endlist %}

### Returned Data

{% list tabs %}

- Single Event

    #|
    || **Name**
    `type` | **Description** ||
    || **result**
    [`integer`](../../data-types.md) | Identifier of the updated event. ||
    |#

- Recurring Event

    #|
    || **Name**
    `type` | **Description** ||
    || **result**
    [`object`](../../data-types.md) | Root element of the response. ||
    || **originalDate**
    [`datetime`](../../data-types.md) | Start date of the new series of the recurring event. ||
    || **originalDavXmlId**
    [`string`](../../data-types.md) | Event identifier for synchronization. ||
    || **instanceTz**
    [`string`](../../data-types.md) | Event timezone. ||
    || **recEventId**
    [`integer`](../../data-types.md) | Identifier of the new series of the recurring event. ||
    || **id**
    [`integer`](../../data-types.md) | Identifier of the previous series of the recurring event. ||
    |#

{% endlist %}

## Error Handling

HTTP status: **400**

```json
{
    "error": "",
    "error_description": "The required parameter "ownerId" for the method "calendar.event.update" is not set."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Error Message** | **Description** ||
|| Empty string | The required parameter "id" for the method "calendar.event.update" is not set. | The required parameter `id` is not provided. ||
|| Empty string | The required parameter "ownerId" for the method "calendar.event.update" is not set. | The required parameter `ownerId` is not provided. ||
|| Empty string | The required parameter "type" for the method "calendar.event.update" is not set. | The required parameter `type` is not provided. ||
|| Empty string | Invalid value for the "name" parameter. | An incorrect data format is provided in the `name` field. ||
|| Empty string | Invalid value for the "description" parameter. | An incorrect data format is provided in the `description` field. ||
|| Empty string | Access denied. | Creating events in the specified calendar is prohibited. ||
|| Empty string | You have specified an invalid calendar section ID or the user does not have access to it. | An identifier of an inaccessible or non-existent calendar is provided. ||
|| Empty string | An invalid type of editing for the recurring event is specified. | An incorrect value is provided in the `recurrence_mode` field. ||
|| Empty string | The event's CRM links list must be an array. | An incorrect data format is provided in the `crm_fields` field. ||
|| Empty string | An error occurred while modifying the event. | Another error. ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}