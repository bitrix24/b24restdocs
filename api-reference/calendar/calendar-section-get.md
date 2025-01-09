# Get Calendar List calendar.section.get

> Scope: [`calendar`](../scopes/permissions.md)
>
> Who can execute the method: any user

This method retrieves a list of calendars.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **type***
[`string`](../data-types.md) | Calendar type: 
- `user` — user calendar
- `group` — group calendar
- `company_calendar` — company calendar 
- `location` — conference room calendar. Used for booking time in the conference room calendar through a third-party application
- other types, including custom ||
|| **ownerId***
[`integer`](../data-types.md) | Identifier of the calendar owner.

For the `location` calendar type, the `ownerId` parameter must be set to `0` ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"type":"user","ownerId":1}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/calendar.section.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"type":"user","ownerId":1,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/calendar.section.get
    ```

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

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'calendar.section.get',
        [
            'type' => 'user',
            'ownerId' => 1
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
[`object`](../data-types.md) | Object with [calendar export parameters](#export)
 ||
|| **CAL_TYPE**
[`string`](../data-types.md) | Calendar type ||
|| **OWNER_ID**
[`string`](../data-types.md) | Identifier of the calendar owner. 

For the user calendar type `user`, this field contains the user identifier. For the group calendar `group`, it contains the group identifier ||
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
[`object`](../data-types.md) | Object containing access data for the calendar. 

The object key is the access permission identifier. You can get the name of the access permission using the [access.name](../common/system/access-name.md) method. Determine access permissions for the current user using the [user.access](../common/users/user-access.md) method.

The object value contains the numerical identifier of the access permission. Access permission identifiers differ across different accounts. Currently, only the portal administrator in the on-premise version of Bitrix24 can retrieve all identifiers. ||
|| **IS_COLLAB**
[`boolean`](../data-types.md) | Flag indicating whether the calendar belongs to a collaboration ||
|| **PERM**
[`object`](../data-types.md) | Object [access permissions](#perm) for the current user to the calendar ||
|#

#### EXPORT Object {#export}

#|
|| **Name**
`type` | **Description** ||
|| **ALLOW**
[`boolean`](../data-types.md) | Calendar export is allowed ||
|#

#### PERM Object {#perm}

#|
|| **Name**
`type` | **Description** ||
|| **view_time**
[`boolean`](../data-types.md) | View calendar event times ||
|| **view_title**
[`boolean`](../data-types.md) | View calendar event titles ||
|| **view_full**
[`boolean`](../data-types.md) | Full access to event information in the calendar ||
|| **add**
[`boolean`](../data-types.md) | Add events to the calendar ||
|| **edit**
[`boolean`](../data-types.md) | Edit events in the calendar ||
|| **edit_section**
[`boolean`](../data-types.md) | Edit the calendar ||
|| **access**
[`boolean`](../data-types.md) | Full access to the calendar ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "The required parameter 'type' for the method 'calendar.section.get' is not set"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Error Message** | **Description** ||
|| Empty string | The required parameter 'type' for the method 'calendar.section.get' is not set | The required parameter `type` was not provided ||
|| Empty string | The required parameter 'ownerId' for the method 'calendar.section.get' is not set | The required parameter `ownerId` was not provided and the `type` parameter is not equal to `user` ||
|| Empty string | Access denied | Access to the method is prohibited for external users ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./calendar-section-add.md)
- [{#T}](./calendar-section-update.md)
- [{#T}](./calendar-section-delete.md)