# Provide the ability to select resource bookings calendar.resource.booking.list

> Scope: [`calendar`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `calendar.resource.booking.list` allows you to select resource bookings.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **filter***
[`object`](../../data-types.md) | Filter fields. ||
|| **resourceTypeIdList***
[`array`](../../data-types.md) | A list of resource identifiers that can be selected using the method `calendar.resource.list` ||
|| **from**
[`date`](../../data-types.md) | Start date of the period. ||
|| **to**
[`date`](../../data-types.md) | End date of the period. ||
|| **resourceIdList***
[`array`](../../data-types.md) | These IDs are taken from the UF field value of type resourcebooking in CRM entities LEAD|DEAL ||
|#

## Examples

**First option:** to assess the bookings (availability) of specific resources over a certain period. This can be used to create custom views of availability or for use in logic.

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'calendar.resource.booking.list',
        {
            filter: {
                resourceTypeIdList: [10852, 10888, 10873, 10871, 10853],
                from: '2024-06-20',
                to: '2024-08-20',
            }
        }
    );
    ```

{% endlist %}

**Second option:** the ability to select bookings by their IDs (these are the values of the UF field associated with the CRM entity).

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'calendar.resource.booking.list',
        {
            filter: {
                resourceIdList: [10, 18, 17]
            }
        }
    );
    ```

{% endlist %}

{% include [Note on examples](../../../_includes/examples.md) %}

## Response Handling

HTTP status: **200**

```json
{
  "result": [
    {
      "ID": "1408",
      "PARENT_ID": "1408",
      "DELETED": "N",
      "CAL_TYPE": "resource",
      "OWNER_ID": "0",
      "NAME": "Booking",
      "DATE_FROM": "12/20/2024 00:00:00",
      "DATE_TO": "12/21/2024 00:00:00",
      "TZ_FROM": "Europe/Berlin",
      "TZ_TO": "Europe/Berlin",
      "TZ_OFFSET_FROM": "7200",
      "TZ_OFFSET_TO": "7200",
      "DATE_FROM_TS_UTC": "1734652800",
      "DATE_TO_TS_UTC": "1734739200",
      "DT_SKIP_TIME": "Y",
      "DT_LENGTH": 172800,
      "EVENT_TYPE": "#resourcebooking#",
      "CREATED_BY": "1",
      "DATE_CREATE": "12/18/2024 13:55:35",
      "TIMESTAMP_X": "12/18/2024 13:55:35",
      "DESCRIPTION": "Service: some",
      "IS_MEETING": false,
      "MEETING_STATUS": "Y",
      "MEETING_HOST": "0",
      "VERSION": "1",
      "SECTION_ID": "198",
      "DATE_FROM_FORMATTED": "Fri Dec 20 2024",
      "DATE_TO_FORMATTED": "Sat Dec 21 2024",
      "SECT_ID": "198",
      "RESOURCE_BOOKING_ID": "1"
    },
    {
      "ID": "1409",
      ...
    }
  ],
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
[`array`](../../data-types.md) | Array of booking objects ||
|| **ID**
[`string`](../../data-types.md) | Booking identifier ||
|| **PARENT_ID**
[`string`](../../data-types.md) | For the booking object, always equal to the `ID` field ||
|| **DELETED**
[`string`](../../data-types.md) | Flag indicating whether the booking is deleted. Possible values:
- `Y` — booking deleted
- `N` — booking not deleted  ||
|| **CAL_TYPE**
[`string`](../../data-types.md) | Type of calendar in which the booking is located ||
|| **OWNER_ID**
[`string`](../../data-types.md) | For the booking object, always equal to '0' ||
|| **NAME**
[`string`](../../data-types.md) | Name of the booking ||
|| **DATE_FROM**
[`datetime`](../../data-types.md) | Start date of the booking ||
|| **DATE_TO**
[`datetime`](../../data-types.md) | End date of the booking ||
|| **TZ_FROM**
[`string`](../../data-types.md) | Timezone of the start date of the booking ||
|| **TZ_TO**
[`string`](../../data-types.md) | Timezone of the end date of the booking ||
|| **TZ_OFFSET_FROM**
[`string`](../../data-types.md) | Time offset of the start of the booking relative to UTC in seconds ||
|| **TZ_OFFSET_TO**
[`string`](../../data-types.md) | Time offset of the end of the booking relative to UTC in seconds ||
|| **DATE_FROM_TS_UTC**
[`string`](../../data-types.md) | Start date and time of the booking in UTC in timestamp format ||
|| **DATE_TO_TS_UTC**
[`string`](../../data-types.md) | End date and time of the booking in UTC in timestamp format ||
|| **DT_SKIP_TIME**
[`string`](../../data-types.md) | Flag indicating that the booking lasts all day. Possible values:
- `Y` — all day
- `N` — not all day ||
|| **DT_LENGTH**
[`integer`](../../data-types.md) | Duration of the booking in seconds ||
|| **EVENT_TYPE**
[`string`](../../data-types.md) | Type of booking ||
|| **CREATED_BY**
[`string`](../../data-types.md) | Identifier of the user who created the booking ||
|| **DATE_CREATE**
[`datetime`](../../data-types.md) | Creation date of the booking ||
|| **TIMESTAMP_X**
[`datetime`](../../data-types.md) | Modification date of the booking ||
|| **DESCRIPTION**
[`string`](../../data-types.md) | Description of the booking ||
|| **IS_MEETING**
[`boolean`](../../data-types.md) | For the booking object, always false ||
|| **MEETING_STATUS**
[`string`](../../data-types.md) | For the booking object, always 'Y' ||
|| **MEETING_HOST**
[`string`](../../data-types.md) | For the booking object, always '0' ||
|| **VERSION**
[`string`](../../data-types.md) | Version of the booking changes ||
|| **SECTION_ID**
[`string`](../../data-types.md) | Identifier of the resource in which the booking is located ||
|| **DATE_FROM_FORMATTED**
[`string`](../../data-types.md) | Formatted start date of the booking ||
|| **DATE_TO_FORMATTED**
[`string`](../../data-types.md) | Formatted end date of the booking ||
|| **SECT_ID**
[`string`](../../data-types.md) | Identifier of the resource in which the booking is located ||
|| **RESOURCE_BOOKING_ID**
[`integer`](../../data-types.md) | Booking identifier ||
|#

## Error Handling

HTTP status: **400**

```json
{
  "error": "",
  "error_description": "The required parameter \"filter['resourceTypeIdList']\" is not set for the method \"calendar.resource.booking.list\""
}
```
{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Error Message** | **Description** ||
|| Empty string | Access denied | Access to the method is prohibited for external users ||
|| Empty string | The required parameter "filter['resourceTypeIdList']" is not set for the method "calendar.resource.booking.list" | None of the required parameters `resourceTypeIdList` or `resourceIdList` were provided ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}