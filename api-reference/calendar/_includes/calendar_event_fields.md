#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Object with event fields ||
|| **ID**
[`string`](../../data-types.md) | Event identifier ||
|| **PARENT_ID**
[`string`](../../data-types.md) | Identifier of the parent event ||
|| **DELETED**
[`string`](../../data-types.md) | Flag indicating whether the event is deleted. Possible values:
- `Y` — event is deleted
- `N` — event is not deleted ||
|| **CAL_TYPE**
[`string`](../../data-types.md) | Type of calendar in which the event is located ||
|| **OWNER_ID**
[`string`](../../data-types.md) | Identifier of the calendar owner:
- `id` of the user for the `user` calendar type
- `id` of the group for the `group` calendar type ||
|| **NAME**
[`string`](../../data-types.md) | Event name ||
|| **DATE_FROM**
[`datetime`](../../data-types.md) | Start date of the event ||
|| **DATE_TO**
[`datetime`](../../data-types.md) | End date of the event ||
|| **ORIGINAL_DATE_FROM**
[`datetime`](../../data-types.md) | Start date of the original event for recurring events ||
|| **TZ_FROM**
[`string`](../../data-types.md) | Time zone of the event start date ||
|| **TZ_TO**
[`string`](../../data-types.md) | Time zone of the event end date ||
|| **TZ_OFFSET_FROM**
[`string`](../../data-types.md) | Time offset of the event start time from UTC in seconds ||
|| **TZ_OFFSET_TO**
[`string`](../../data-types.md) | Time offset of the event end time from UTC in seconds ||
|| **DATE_FROM_TS_UTC**
[`string`](../../data-types.md) | Start date and time of the event in UTC in timestamp format ||
|| **DATE_TO_TS_UTC**
[`string`](../../data-types.md) | End date and time of the event in UTC in timestamp format ||
|| **DT_SKIP_TIME**
[`string`](../../data-types.md) | Flag indicating that the event lasts all day. Possible values: 
- `Y` — all day
- `N` — not all day ||
|| **DT_LENGTH**
[`integer`](../../data-types.md) | Duration of the event in seconds ||
|| **EVENT_TYPE**
[`string`](../../data-types.md) | Type of event ||
|| **CREATED_BY**
[`string`](../../data-types.md) | Identifier of the user who created the event ||
|| **DATE_CREATE**
[`datetime`](../../data-types.md) | Date the event was created ||
|| **TIMESTAMP_X**
[`datetime`](../../data-types.md) | Date the event was modified ||
|| **DESCRIPTION**
[`string`](../../data-types.md) | Description of the event ||
|| **PRIVATE_EVENT**
[`string`](../../data-types.md) | Mark indicating that the event is private. Possible values:

- `Y` — private
- `N` — not private ||
|| **ACCESSIBILITY**
[`string`](../../data-types.md) | Accessibility of event participants ||
|| **IMPORTANCE**
[`string`](../../data-types.md) | Importance of the event ||
|| **IS_MEETING**
[`boolean`](../../data-types.md) | Indicator of a meeting with event participants. Possible values: 

- `Y` — meeting with participants
- `N` — meeting without participants ||
|| **MEETING_STATUS**
[`string`](../../data-types.md) | Status of participation in the event. Possible values:
- `Y` — accepted
- `N` — declined
- `Q` — invited but not yet responded
- `H` — event organizer ||
|| **MEETING_HOST**
[`string`](../../data-types.md) | Identifier of the user hosting the event ||
|| **MEETING**
[`object`](../../data-types.md) | Object describing [meeting settings](#meeting) ||
|| **LOCATION**
[`string`](../../data-types.md) | Identifier or name of the event location ||
|| **REMIND**
[`array`](../../data-types.md) | Array of objects describing [event reminders](#remind) ||
|| **COLOR**
[`string`](../../data-types.md) | Background color of the event ||
|| **RRULE**
[`object`](../../data-types.md) | Recurrence of the event in the form of an [object](#rrule) in terms of the iCalendar standard ||
|| **EXDATE**
[`string`](../../data-types.md) | List of exception dates from the recurrence rule ||
|| **DAV_XML_ID**
[`string`](../../data-types.md) | Synchronization identifier ||
|| **G_EVENT_ID**
[`string`](../../data-types.md) | Synchronization identifier ||
|| **CAL_DAV_LABEL**
[`string`](../../data-types.md) | Synchronization identifier ||
|| **VERSION**
[`string`](../../data-types.md) | Version of event changes ||
|| **ATTENDEES_CODES**
[`array`](../../data-types.md) | Identifiers of event participants ||
|| **RECURRENCE_ID**
[`string`](../../data-types.md) | Identifier of the original event when editing only the current one ||
|| **RELATIONS**
[`object`](../../data-types.md) | Object for recurring events with information about relationships to the [original event](#relations) ||
|| **SECTION_ID**
[`string`](../../data-types.md) | Identifier of the calendar in which the event is located ||
|| **SYNC_STATUS**
[`string`](../../data-types.md) | Synchronization status of the event ||
|| **UF_CRM_CAL_EVENT**
[`array`](../../data-types.md) | Array of identifiers of CRM entities linked to the event ||
|| **UF_WEBDAV_CAL_EVENT**
[`array`](../../data-types.md) | Array of identifiers of files linked to the event ||
|| **SECTION_DAV_XML_ID**
[`array`](../../data-types.md) | Synchronization identifier of the event calendar ||
|| **DATE_FROM_FORMATTED**
[`string`](../../data-types.md) | Formatted start date of the event ||
|| **DATE_TO_FORMATTED**
[`string`](../../data-types.md) | Formatted end date of the event ||
|| **SECT_ID**
[`string`](../../data-types.md) | Identifier of the calendar in which the event is located ||
|| **ATTENDEE_LIST**
[`array`](../../data-types.md) | Array of objects describing event participants and their participation statuses. The structure of the object is described [below](#attendee_list) ||
|| **COLLAB_ID**
[`integer`](../../data-types.md) | Identifier of the collaboration in which the event was created ||
|| **~RRULE_DESCRIPTION**
[`integer`](../../data-types.md) | Text description of the event recurrence rule ||
|| **attendeesEntityList**
[`array`](../../data-types.md) | Array of objects describing users — [event participants](#attendeesEntityList) ||
|| **~DESCRIPTION**
[`string`](../../data-types.md) | Description of the event ||
|| **~USER_OFFSET_FROM**
[`integer`](../../data-types.md) | Time offset of the event start time from the current user's time zone ||
|| **~USER_OFFSET_TO**
[`integer`](../../data-types.md) | Time offset of the event end time from the current user's time zone ||
|#

### MEETING Object {#meeting}

#|
|| **Name**
`type` | **Description** ||
|| **HOST_NAME**
[`string`](../../data-types.md) | Name of the user hosting the event ||
|| **NOTIFY**
[`boolean`](../../data-types.md) | Flag for notifying about confirmation or decline of participants ||
|| **REINVITE**
[`boolean`](../../data-types.md) | Flag for requesting re-confirmation of participation when editing the event ||
|| **ALLOW_INVITE**
[`boolean`](../../data-types.md) | Flag allowing participants to invite others to the event ||
|| **HIDE_GUESTS**
[`boolean`](../../data-types.md) | Flag for hiding the list of participants ||
|| **MEETING_CREATOR**
[`integer`](../../data-types.md) | Identifier of the event creator ||
|| **LANGUAGE_ID**
[`string`](../../data-types.md) | Language identifier for event notifications ||
|| **MAIL_FROM**
[`string`](../../data-types.md) | Sender's address for notifications ||
|#

### REMIND Object {#remind}

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

### RRULE Object {#rrule}

#|
|| **Name**
`type` | **Description** ||
|| **FREQ**
[`string`](../../data-types.md) | Frequency of recurrence
- `DAILY` — daily
- `WEEKLY` — weekly
- `MONTHLY` — monthly
- `YEARLY` — yearly
||
||**BYDAY**
[`object`](../../data-types.md) | Days of the week
- `SU` — Sunday
- `MO` — Monday
- `TU` — Tuesday
- `WE` — Wednesday
- `TH` — Thursday
- `FR` — Friday
- `SA` — Saturday ||
|| **INTERVAL**
[`integer`](../../data-types.md) | Interval between recurrences ||
|| **UNTIL**
[`date`](../../data-types.md) | End date of recurrences ||
|| **~UNTIL**
[`date`](../../data-types.md) | End date of recurrences. Technical field ||
|| **UNTIL_TS**
[`integer`](../../data-types.md) | End date of recurrences in timestamp format ||
|#

### RELATIONS Object {#relations}

#|
|| **Name**
`type` | **Description** ||
|| **ORIGINAL_RECURSION_ID**
[`integer`](../../data-types.md) | Identifier of the original event for recurring events created when editing ||
|| **COMMENT_XML_ID**
[`string`](../../data-types.md) | Identifier of the original event for single events created when editing from recurring events ||
|#

### ATTENDEE_LIST Objects {#attendee_list}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Identifier of the user ||
|| **entryId**
[`string`](../../data-types.md) | Identifier of the event ||
|| **status**
[`string`](../../data-types.md) | Status of the event participant. Possible values:
- `Y` — accepted
- `N` — declined
- `Q` — invited but not yet responded
- `H` — event organizer ||
|#

### attendeesEntityList Object {#attendeesEntityList}

#|
|| **Name**
`type` | **Description** ||
|| **entityId**
[`string`](../../data-types.md) | Type of the event participant entity ||
|| **id**
[`integer`](../../data-types.md) | Identifier of the event participant ||
|#