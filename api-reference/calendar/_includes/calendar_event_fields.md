#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](/api-reference/data-types.html) | Object with event fields ||
|| **ID**
[`string`](/api-reference/data-types.html) | Event identifier ||
|| **PARENT_ID**
[`string`](/api-reference/data-types.html) | Identifier of the parent event ||
|| **DELETED**
[`string`](/api-reference/data-types.html) | Flag indicating whether the event is deleted. Possible values:
- `Y` — event is deleted
- `N` — event is not deleted ||
|| **CAL_TYPE**
[`string`](/api-reference/data-types.html) | Type of calendar in which the event is located ||
|| **OWNER_ID**
[`string`](/api-reference/data-types.html) | Identifier of the calendar owner:
- `id` of the user for the calendar type `user`
- `id` of the group for the calendar type `group` ||
|| **NAME**
[`string`](/api-reference/data-types.html) | Event name ||
|| **DATE_FROM**
[`datetime`](/api-reference/data-types.html) | Start date of the event ||
|| **DATE_TO**
[`datetime`](/api-reference/data-types.html) | End date of the event ||
|| **ORIGINAL_DATE_FROM**
[`datetime`](/api-reference/data-types.html) | Start date of the original event for recurring events ||
|| **TZ_FROM**
[`string`](/api-reference/data-types.html) | Timezone of the event start date ||
|| **TZ_TO**
[`string`](/api-reference/data-types.html) | Timezone of the event end date ||
|| **TZ_OFFSET_FROM**
[`string`](/api-reference/data-types.html) | Time offset of the event start time relative to UTC in seconds ||
|| **TZ_OFFSET_TO**
[`string`](/api-reference/data-types.html) | Time offset of the event end time relative to UTC in seconds ||
|| **DATE_FROM_TS_UTC**
[`string`](/api-reference/data-types.html) | Start date and time of the event in UTC in timestamp format ||
|| **DATE_TO_TS_UTC**
[`string`](/api-reference/data-types.html) | End date and time of the event in UTC in timestamp format ||
|| **DT_SKIP_TIME**
[`string`](/api-reference/data-types.html) | Flag indicating that the event lasts all day. Possible values:
- `Y` — all day
- `N` — not all day ||
|| **DT_LENGTH**
[`integer`](/api-reference/data-types.html) | Duration of the event in seconds ||
|| **EVENT_TYPE**
[`string`](/api-reference/data-types.html) | Type of event ||
|| **CREATED_BY**
[`string`](/api-reference/data-types.html) | Identifier of the user who created the event ||
|| **DATE_CREATE**
[`datetime`](/api-reference/data-types.html) | Date the event was created ||
|| **TIMESTAMP_X**
[`datetime`](/api-reference/data-types.html) | Date the event was modified ||
|| **DESCRIPTION**
[`string`](/api-reference/data-types.html) | Description of the event ||
|| **PRIVATE_EVENT**
[`string`](/api-reference/data-types.html) | Mark indicating that the event is private. Possible values:

- `Y` — private
- `N` — not private ||
|| **ACCESSIBILITY**
[`string`](/api-reference/data-types.html) | Availability of event participants ||
|| **IMPORTANCE**
[`string`](/api-reference/data-types.html) | Importance of the event ||
|| **IS_MEETING**
[`boolean`](/api-reference/data-types.html) | Indicator of a meeting with event participants. Possible values:

- `Y` — meeting with participants
- `N` — meeting without participants ||
|| **MEETING_STATUS**
[`string`](/api-reference/data-types.html) | Status of participation in the event. Possible values:
- `Y` — accepted
- `N` — declined
- `Q` — invited but not yet responded
- `H` — event organizer ||
|| **MEETING_HOST**
[`string`](/api-reference/data-types.html) | Identifier of the user hosting the event ||
|| **MEETING**
[`object`](/api-reference/data-types.html) | Object describing [meeting settings](#meeting) ||
|| **LOCATION**
[`string`](/api-reference/data-types.html) | Identifier or name of the event location ||
|| **REMIND**
[`array`](/api-reference/data-types.html) | Array of objects describing [event reminders](#remind) ||
|| **COLOR**
[`string`](/api-reference/data-types.html) | Background color of the event ||
|| **RRULE**
[`object`](/api-reference/data-types.html) | Recurrence of the event in the form of an [object](#rrule) in terms of the iCalendar standard ||
|| **EXDATE**
[`string`](/api-reference/data-types.html) | List of exception dates from the recurrence rule ||
|| **DAV_XML_ID**
[`string`](/api-reference/data-types.html) | Synchronization identifier ||
|| **G_EVENT_ID**
[`string`](/api-reference/data-types.html) | Synchronization identifier ||
|| **CAL_DAV_LABEL**
[`string`](/api-reference/data-types.html) | Synchronization identifier ||
|| **VERSION**
[`string`](/api-reference/data-types.html) | Version of event changes ||
|| **ATTENDEES_CODES**
[`array`](/api-reference/data-types.html) | Identifiers of event participants ||
|| **RECURRENCE_ID**
[`string`](/api-reference/data-types.html) | Identifier of the original event when editing only the current one ||
|| **RELATIONS**
[`object`](/api-reference/data-types.html) | Object for recurring events with information about relationships to the [original event](#relations) ||
|| **SECTION_ID**
[`string`](/api-reference/data-types.html) | Identifier of the calendar in which the event is located ||
|| **SYNC_STATUS**
[`string`](/api-reference/data-types.html) | Synchronization status of the event ||
|| **UF_CRM_CAL_EVENT**
[`array`](/api-reference/data-types.html) | Array of identifiers of CRM objects linked to the event ||
|| **UF_WEBDAV_CAL_EVENT**
[`array`](/api-reference/data-types.html) | Array of identifiers of files linked to the event ||
|| **SECTION_DAV_XML_ID**
[`array`](/api-reference/data-types.html) | Synchronization identifier of the event calendar ||
|| **DATE_FROM_FORMATTED**
[`string`](/api-reference/data-types.html) | Formatted start date of the event ||
|| **DATE_TO_FORMATTED**
[`string`](/api-reference/data-types.html) | Formatted end date of the event ||
|| **SECT_ID**
[`string`](/api-reference/data-types.html) | Identifier of the calendar in which the event is located ||
|| **ATTENDEE_LIST**
[`array`](/api-reference/data-types.html) | Array of objects describing event participants and their participation statuses. The structure of the object is described [below](#attendee_list) ||
|| **COLLAB_ID**
[`integer`](/api-reference/data-types.html) | Identifier of the collaboration in which the event was created ||
|| **~RRULE_DESCRIPTION**
[`string`](/api-reference/data-types.html) | Text description of the event recurrence rule ||
|| **attendeesEntityList**
[`array`](/api-reference/data-types.html) | Array of objects describing users — [event participants](#attendeesEntityList) ||
|| **~DESCRIPTION**
[`string`](/api-reference/data-types.html) | Description of the event ||
|| **~USER_OFFSET_FROM**
[`integer`](/api-reference/data-types.html) | Time offset of the event start time relative to the current user's timezone ||
|| **~USER_OFFSET_TO**
[`integer`](/api-reference/data-types.html) | Time offset of the event end time relative to the current user's timezone ||
|#

### MEETING Object {#meeting}

#|
|| **Name**
`type` | **Description** ||
|| **HOST_NAME**
[`string`](/api-reference/data-types.html) | Name of the user hosting the event ||
|| **NOTIFY**
[`boolean`](/api-reference/data-types.html) | Flag for notifying about confirmation or decline of participants ||
|| **REINVITE**
[`boolean`](/api-reference/data-types.html) | Flag for requesting re-confirmation of participation when editing the event ||
|| **ALLOW_INVITE**
[`boolean`](/api-reference/data-types.html) | Flag allowing participants to invite others to the event ||
|| **HIDE_GUESTS**
[`boolean`](/api-reference/data-types.html) | Flag for hiding the list of participants ||
|| **MEETING_CREATOR**
[`integer`](/api-reference/data-types.html) | Identifier of the event creator ||
|| **LANGUAGE_ID**
[`string`](/api-reference/data-types.html) | Language identifier for event notifications ||
|| **MAIL_FROM**
[`string`](/api-reference/data-types.html) | Sender's address for notifications ||
|#

### REMIND Object {#remind}

#|
|| **Name**
`type` | **Description** ||
|| **type**
[`string`](/api-reference/data-types.html) | Time type of the reminder
- `min` — minutes
- `hour` – hours
- `day` — days ||
|| **count**
[`integer`](/api-reference/data-types.html) | Numerical value of the time interval ||
|#

### RRULE Object {#rrule}

#|
|| **Name**
`type` | **Description** ||
|| **FREQ**
[`string`](/api-reference/data-types.html) | Frequency of recurrence
- `DAILY` — daily
- `WEEKLY` — weekly
- `MONTHLY` — monthly
- `YEARLY` — yearly
||
||**BYDAY**
[`object`](/api-reference/data-types.html) | Days of the week
- `SU` — Sunday
- `MO` — Monday
- `TU` — Tuesday
- `WE` — Wednesday
- `TH` — Thursday
- `FR` — Friday
- `SA` — Saturday ||
|| **INTERVAL**
[`integer`](/api-reference/data-types.html) | Interval between recurrences ||
|| **UNTIL**
[`date`](/api-reference/data-types.html) | End date of recurrences ||
|| **~UNTIL**
[`date`](/api-reference/data-types.html) | End date of recurrences. Technical field ||
|| **UNTIL_TS**
[`integer`](/api-reference/data-types.html) | End date of recurrences in timestamp format ||
|#

### RELATIONS Object {#relations}

#|
|| **Name**
`type` | **Description** ||
|| **ORIGINAL_RECURSION_ID**
[`integer`](/api-reference/data-types.html) | Identifier of the original event for recurring events created when editing ||
|| **COMMENT_XML_ID**
[`string`](/api-reference/data-types.html) | Identifier of the original event for single events created when editing from recurring ones ||
|#

### ATTENDEE_LIST Objects {#attendee_list}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](/api-reference/data-types.html) | Identifier of the user ||
|| **entryId**
[`string`](/api-reference/data-types.html) | Identifier of the event ||
|| **status**
[`string`](/api-reference/data-types.html) | Status of the event participant. Possible values:
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
[`string`](/api-reference/data-types.html) | Event participant object type ||
|| **id**
[`integer`](/api-reference/data-types.html) | Identifier of the event participant ||
|#
