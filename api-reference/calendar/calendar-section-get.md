# Get List of Calendars calendar.section.get

> Scope: [`calendar`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `calendar.section.get` returns a list of calendars. Here and further, section will be referred to as "calendar".

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Parameter** | **Description** ||
|| **type***
[`string`](../data-types.md) | Calendar type: 
- user 
- group 
- company_calendar 
- location 
- other types, including custom. ||
|| **ownerId***
[`integer`](../data-types.md) | Identifier of the calendar owner. ||
|#

The method can be used to book time in a meeting room calendar through a third-party application. In this case, **type** should be set to `location`, and **ownerId** should be `0`.

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'calendar.section.get',
        {
            type: 'user',
            ownerId: 1
        }
    );
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
  "result": [
    {
      "ID": "190",
      "NAME": "New Section",
      "GAPI_CALENDAR_ID": null,
      "DESCRIPTION": "Description for section",
      "COLOR": "#9cbeee",
      "TEXT_COLOR": "#283000",
      "EXPORT": {
        "ALLOW": true
      },
      "CAL_TYPE": "user",
      "OWNER_ID": "1",
      "CREATED_BY": "1",
      "DATE_CREATE": "2024-12-10 06:36:00",
      "TIMESTAMP_X": "2024-12-10 06:36:00",
      "CAL_DAV_CON": null,
      "SYNC_TOKEN": null,
      "PAGE_TOKEN": null,
      "EXTERNAL_TYPE": "local",
      "ACCESS": {
        "D114": 17,
        "G2": 13,
        "U2": 15,
        "U1": 19
      },
      "IS_COLLAB": false,
      "PERM": {
        "view_time": true,
        "view_title": true,
        "view_full": true,
        "add": true,
        "edit": true,
        "edit_section": true,
        "access": true
      }
    },
    {
      "ID": "191",
      ...
    }
    {
      "ID": "192",
      ...
    }
  ],
  "time": {
    "start": 1733828946.418185,
    "finish": 1733828946.650208,
    "duration": 0.23202300071716309,
    "processing": 0.0054471492767333984,
    "date_start": "2024-12-08T11:09:06+00:00",
    "date_finish": "2024-12-08T11:09:06+00:00"
  }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../data-types.md) | Array of calendars ||
|| **ID**
[`string`](../data-types.md) | Calendar identifier ||
|| **NAME**
[`string`](../data-types.md) | Calendar name ||
|| **GAPI_CALENDAR_ID**
[`string`](../data-types.md) | Synchronization identifier ||
|| **DESCRIPTION**
[`string`](../data-types.md) | Calendar description ||
|| **COLOR**
[`string`](../data-types.md) | Calendar color ||
|| **TEXT_COLOR**
[`string`](../data-types.md) | Text color in the calendar ||
|| **EXPORT**
[`object`](../data-types.md) | List of calendar export parameters:
- ALLOW [`boolean`](../data-types.md) - calendar export is allowed ||
|| **CAL_TYPE**
[`string`](../data-types.md) | Calendar type ||
|| **OWNER_ID**
[`string`](../data-types.md) | Identifier of the calendar owner, user (if the calendar type is `user`) or group (if the calendar type is `group`) ||
|| **CREATED_BY**
[`string`](../data-types.md) | Identifier of the calendar creator ||
|| **DATE_CREATE**
[`datetime`](../data-types.md) | Calendar creation date ||
|| **TIMESTAMP_X**
[`datetime`](../data-types.md) | Calendar modification date ||
|| **CAL_DAV_CON**
[`string`](../data-types.md) | Synchronization identifier ||
|| **SYNC_TOKEN**
[`string`](../data-types.md) | Synchronization identifier ||
|| **PAGE_TOKEN**
[`string`](../data-types.md) | Synchronization identifier ||
|| **EXTERNAL_TYPE**
[`string`](../data-types.md) | Provider type for synchronization ||
|| **ACCESS**
[`object`](../data-types.md) | Access data object for the calendar:
- key contains the access permission identifier
  - for more details, see access names [access.name](../common/system/access-name.md);
  - for more details, see access permissions for the current user [user.access](../common/users/user-access.md)
- value contains the identifier of the access permission on the current account, currently specific identifiers can only be viewed by the account administrator.||
|| **IS_COLLAB**
[`boolean`](../data-types.md) | Flag indicating if the calendar belongs to a collaboration ||
|| **PERM**
[`object`](../data-types.md) | Object of the current user's access permissions to the calendar:
- view_time [`boolean`](../data-types.md) - ability to view the time of calendar events;
- view_title [`boolean`](../data-types.md) - ability to view the title of calendar events;
- view_full [`boolean`](../data-types.md) - ability to have full access to information about calendar events;
- add [`boolean`](../data-types.md) - ability to add events to the calendar;
- edit [`boolean`](../data-types.md) - ability to edit events in the calendar;
- edit_section [`boolean`](../data-types.md) - ability to edit the calendar;
- access [`boolean`](../data-types.md) - ability to have full access to the calendar ||
|#

## Error Handling

HTTP status: **400**

```json
{
  "error": "",
  "error_description": "The required parameter \"type\" for the method \"calendar.section.get\" is not set"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Error Message** | **Description** ||
|| Empty string | The required parameter "type" for the method "calendar.section.get" is not set | The required parameter `type` was not provided ||
|| Empty string | The required parameter "ownerId" for the method "calendar.section.get" is not set | The required parameter `ownerId` was not provided and the `type` parameter is not equal to 'user' ||
|| Empty string | Access denied | Access to the method is denied for external users ||
|#

{% include [system errors](../../_includes/system-errors.md) %}