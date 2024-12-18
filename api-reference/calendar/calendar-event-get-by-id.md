# View Calendar Event by ID calendar.event.getbyid

> Scope: [`calendar`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `calendar.event.getbyid` returns a calendar event by its ID.

## Method Parameters

{% include [Note on Required Parameters](../../_includes/required.md) %}

#|
|| **Parameter** | **Description** ||
|| **id***
[`integer`](../data-types.md) | The ID of the event. ||
|#

{% include [Note on Parameters](../../_includes/required.md) %}

## Example

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'calendar.event.getbyid',
        {
            id: 324
        }
    );
    ```

{% endlist %}

{% include [Note on Examples](../../_includes/examples.md) %}

## Response Handling

HTTP Status: **200**

```json
{
  "result": {
    "ID": "1265",
    "PARENT_ID": "1265",
    "DELETED": "N",
    "CAL_TYPE": "user",
    "OWNER_ID": "1",
    "NAME": "Event Name",
    "DATE_FROM": "12/11/2024 05:59:00 pm",
    "DATE_TO": "12/11/2024 06:59:00 pm",
    "ORIGINAL_DATE_FROM": null,
    "TZ_FROM": "Europe/Kaliningrad",
    "TZ_TO": "Europe/Kaliningrad",
    "TZ_OFFSET_FROM": "7200",
    "TZ_OFFSET_TO": "7200",
    "DATE_FROM_TS_UTC": "1733932740",
    "DATE_TO_TS_UTC": "1733936340",
    "DT_SKIP_TIME": "N",
    "DT_LENGTH": 3600,
    "EVENT_TYPE": null,
    "CREATED_BY": "1",
    "DATE_CREATE": "12/05/2024 01:48:41 pm",
    "TIMESTAMP_X": "12/05/2024 01:48:41 pm",
    "DESCRIPTION": "Description for event",
    "PRIVATE_EVENT": "",
    "ACCESSIBILITY": "free",
    "IMPORTANCE": "normal",
    "IS_MEETING": true,
    "MEETING_STATUS": "H",
    "MEETING_HOST": "1",
    "MEETING": {
      "HOST_NAME": "User Name",
      "NOTIFY": false,
      "REINVITE": false,
      "ALLOW_INVITE": false,
      "HIDE_GUESTS": false,
      "MEETING_CREATOR": 1,
      "LANGUAGE_ID": "de",
      "MAIL_FROM": ""
    },
    "LOCATION": "test location",
    "REMIND": [
      {
        "type": "min",
        "count": 50
      }
    ],
    "COLOR": "#9dcf00",
    "RRULE": {
      "FREQ": "WEEKLY",
      "BYDAY": {
        "MO": "MO",
        "WE": "WE"
      },
      "INTERVAL": 1,
      "UNTIL": "12/24/2024",
      "~UNTIL": "12/24/2024",
      "UNTIL_TS": 1734998400
    },
    "EXDATE": "11/28/2024;12/05/2024;12/12/2024;12/19/2024;12/26/2024",
    "DAV_XML_ID": "20241211T155900Z-534185204b362e9be7e261e92ccd9078@b24evo.lan",
    "G_EVENT_ID": "",
    "DAV_EXCH_LABEL": "",
    "CAL_DAV_LABEL": "",
    "VERSION": "1",
    "ATTENDEES_CODES": [
      "U1"
    ],
    "RECURRENCE_ID": 1272,
    "RELATIONS": {
      "ORIGINAL_RECURSION_ID": 1271,
      "COMMENT_XML_ID": "EVENT_1271_12/23/2024"
    },
    "SECTION_ID": "4",
    "SYNC_STATUS": null,
    "UF_CRM_CAL_EVENT": [
      "CO_1",
      "L_5"
    ],
    "UF_WEBDAV_CAL_EVENT": false,
    "SECTION_DAV_XML_ID": null,
    "DATE_FROM_FORMATTED": "Wed Dec 11 2024 17:59:00",
    "DATE_TO_FORMATTED": "Wed Dec 11 2024 18:59:00",
    "SECT_ID": "4",
    "ATTENDEE_LIST": [
      {
        "id": 1,
        "entryId": "1265",
        "status": "H"
      }
    ],
    "COLLAB_ID": null,
    "~RRULE_DESCRIPTION": "every week on: Mon, Wed, from 12/11/2024 to 12/24/2024",
    "attendeesEntityList": [
      {
        "entityId": "user",
        "id": 1
      }
    ],
    "~DESCRIPTION": "Description for event",
    "~USER_OFFSET_FROM": 7200,
    "~USER_OFFSET_TO": 7200
  },
  "time": {
    "start": 1733406529.495513,
    "finish": 1733406529.744687,
    "duration": 0.2491741180419922,
    "processing": 0.0327458381652832,
    "date_start": "2024-12-05T13:48:49+00:00",
    "date_finish": "2024-12-05T13:48:49+00:00"
  }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | All available fields of the event object ||
|| **ID**
[`string`](../data-types.md) | The ID of the event ||
|| **PARENT_ID**
[`string`](../data-types.md) | The ID of the parent event ||
|| **DELETED**
[`string`](../data-types.md) | Flag indicating whether the event is deleted [Y\|N] ||
|| **CAL_TYPE**
[`string`](../data-types.md) | The type of calendar in which the event is located ||
|| **OWNER_ID**
[`string`](../data-types.md) | The ID of the user (if the calendar type is `user`) or group (if the calendar type is `group`) ||
|| **NAME**
[`string`](../data-types.md) | The name of the event ||
|| **DATE_FROM**
[`datetime`](../data-types.md) | The start date of the event ||
|| **DATE_TO**
[`datetime`](../data-types.md) | The end date of the event ||
|| **ORIGINAL_DATE_FROM**
[`datetime`](../data-types.md) | The start date of the original event (for recurring events) ||
|| **TZ_FROM**
[`string`](../data-types.md) | The timezone of the event's start date ||
|| **TZ_TO**
[`string`](../data-types.md) | The timezone of the event's end date ||
|| **TZ_OFFSET_FROM**
[`string`](../data-types.md) | The offset of the event's start time from UTC in seconds ||
|| **TZ_OFFSET_TO**
[`string`](../data-types.md) | The offset of the event's end time from UTC in seconds ||
|| **DATE_FROM_TS_UTC**
[`string`](../data-types.md) | The timestamp of the event's start in UTC ||
|| **DATE_TO_TS_UTC**
[`string`](../data-types.md) | The timestamp of the event's end in UTC ||
|| **DT_SKIP_TIME**
[`string`](../data-types.md) | Flag indicating that the event lasts all day [Y\|N] ||
|| **DT_LENGTH**
[`integer`](../data-types.md) | The duration of the event in seconds ||
|| **EVENT_TYPE**
[`string`](../data-types.md) | The type of event ||
|| **CREATED_BY**
[`string`](../data-types.md) | The ID of the user who created the event ||
|| **DATE_CREATE**
[`datetime`](../data-types.md) | The creation date of the event ||
|| **TIMESTAMP_X**
[`datetime`](../data-types.md) | The modification date of the event ||
|| **DESCRIPTION**
[`string`](../data-types.md) | The description of the event ||
|| **PRIVATE_EVENT**
[`string`](../data-types.md) | Flag for a private event [Y\|N] ||
|| **ACCESSIBILITY**
[`string`](../data-types.md) | The accessibility of the event participants ||
|| **IMPORTANCE**
[`string`](../data-types.md) | The importance of the event ||
|| **IS_MEETING**
[`string`](../data-types.md) | Flag indicating whether the event is a meeting [Y\|N] ||
|| **MEETING_STATUS**
[`string`](../data-types.md) | The participation status in the event [Y\|N\|Q\|H]
- Y - accepted;
- N - declined;
- Q - invited but not yet responded;
- H - event organizer.||
|| **MEETING_HOST**
[`string`](../data-types.md) | The ID of the user hosting the event ||
|| **MEETING**
[`object`](../data-types.md) | Meeting settings object:
- HOST_NAME [`string`](../data-types.md) - the name of the user hosting the event;
- NOTIFY [`boolean`](../data-types.md) - flag for notifying about participant confirmations/declines;
- REINVITE [`boolean`](../data-types.md) - flag for requesting re-confirmation of participation;
- ALLOW_INVITE [`boolean`](../data-types.md) - flag allowing participants to invite others to the event;
- HIDE_GUESTS [`boolean`](../data-types.md) - flag for hiding the list of participants;
- MEETING_CREATOR [`integer`](../data-types.md) - the ID of the event creator;
- LANGUAGE_ID [`string`](../data-types.md) - the language ID for event notifications;
- MAIL_FROM [`string`](../data-types.md) - the sender's address for notifications.||
|| **LOCATION**
[`string`](../data-types.md) | The ID or name of the event location ||
|| **REMIND**
[`array`](../data-types.md) | An array of reminder objects:
- type [`string`](../data-types.md) - the time type of the reminder (min, hour, day);
- count [`integer`](../data-types.md) - the numerical value of the time interval. ||
|| **COLOR**
[`string`](../data-types.md) | The background color of the event ||
|| **RRULE**
[`object`](../data-types.md) | The recurrence of the event. In terms of the iCalendar standard:
- FREQ [`string`](../data-types.md) - the frequency of recurrence (DAILY, WEEKLY, MONTHLY, YEARLY);
- BYDAY [`object`](../data-types.md) - days of the week ('SU', 'MO', 'TU', 'WE', 'TH', 'FR', 'SA');
- INTERVAL [`integer`](../data-types.md) - the interval between recurrences;
- UNTIL [`date`](../data-types.md) - the end date of recurrences;
- ~UNTIL [`date`](../data-types.md) - the end date of recurrences;
- UNTIL_TS [`integer`](../data-types.md) - the timestamp of the end of recurrences. ||
|| **EXDATE**
[`string`](../data-types.md) | A list of exception dates from the recurrence rule ||
|| **DAV_XML_ID**
[`string`](../data-types.md) | Synchronization ID ||
|| **G_EVENT_ID**
[`string`](../data-types.md) | Synchronization ID ||
|| **CAL_DAV_LABEL**
[`string`](../data-types.md) | Synchronization ID ||
|| **VERSION**
[`string`](../data-types.md) | The version of the event changes ||
|| **ATTENDEES_CODES**
[`array`](../data-types.md) | The IDs of the event participants ||
|| **RECURRENCE_ID**
[`string`](../data-types.md) | The ID of the original event when editing only the current one ||
|| **RELATIONS**
[`object`](../data-types.md) |
- ORIGINAL_RECURSION_ID [`integer`](../data-types.md) - the ID of the original event for recurring events created during editing;
- COMMENT_XML_ID [`string`](../data-types.md) - the ID of the original event for single events created during editing from recurring ones.||
|| **SECTION_ID**
[`string`](../data-types.md) | The ID of the calendar in which the event is located ||
|| **SYNC_STATUS**
[`string`](../data-types.md) | The synchronization status of the event ||
|| **UF_CRM_CAL_EVENT**
[`array`](../data-types.md) | An array of IDs of CRM entities linked to the event ||
|| **UF_WEBDAV_CAL_EVENT**
[`array`](../data-types.md) | An array of IDs of files linked to the event ||
|| **SECTION_DAV_XML_ID**
[`array`](../data-types.md) | The synchronization ID of the event calendar ||
|| **DATE_FROM_FORMATTED**
[`string`](../data-types.md) | The formatted start date of the event ||
|| **DATE_TO_FORMATTED**
[`string`](../data-types.md) | The formatted end date of the event ||
|| **SECT_ID**
[`string`](../data-types.md) | The ID of the calendar in which the event is located ||
|| **ATTENDEE_LIST**
[`array`](../data-types.md) | An array of users participating in the event:
- id [`integer`](../data-types.md) - the ID of the user;
- entryId [`string`](../data-types.md) - the ID of the event;
- status [`string`](../data-types.md) - the status of the event participant [Y\|N\|Q\|H]:
  - Y - accepted;
  - N - declined;
  - Q - invited but not yet responded;
  - H - event organizer.||
|| **COLLAB_ID**
[`integer`](../data-types.md) | The ID of the collaboration in which the event was created ||
|| **~RRULE_DESCRIPTION**
[`integer`](../data-types.md) | Text description of the event recurrence rule ||
|| **attendeesEntityList**
[`array`](../data-types.md) | An array of event participants:
- entityId [`string`](../data-types.md) - the type of entity of the event participant;
- id [`integer`](../data-types.md) - the ID of the event participant.||
|| **~DESCRIPTION**
[`string`](../data-types.md) | The description of the event ||
|| **~USER_OFFSET_FROM**
[`integer`](../data-types.md) | The offset of the event's start time from the current user's timezone ||
|| **~USER_OFFSET_TO**
[`integer`](../data-types.md) | The offset of the event's end time from the current user's timezone ||
|#

## Error Handling

HTTP Status: **400**

```json
{
  "error": "",
  "error_description": "The required parameter \"id\" for the method \"calendar.event.getbyid\" is not set"
}
```
{% include notitle [Error Handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Error Message** | **Description** ||
|| Empty string | The required parameter "id" for the method "calendar.event.getbyid" is not set | The required parameter `id` was not provided ||
|| Empty string | Access denied | Access to the method is prohibited for external users ||
|#

{% include [System Errors](../../_includes/system-errors.md) %}