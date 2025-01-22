# Get Resource Bookings by Filter calendar.resource.booking.list

> Scope: [`calendar`](../../scopes/permissions.md)
>
> Who can execute the method: any user

This method retrieves resource bookings based on a filter.

## Method Parameters

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **filter***
[`object`](../../data-types.md) | Filter fields ||
|#

### Filter Parameter

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **resourceTypeIdList***
[`array`](../../data-types.md) | List of resource identifiers.

Identifiers can be obtained using the [calendar.resource.list](./calendar-resource-list.md) method ||
|| **from**
[`date`](../../data-types.md) | Start date of the period ||
|| **to**
[`date`](../../data-types.md) | End date of the period ||
|| **resourceIdList***
[`array`](../../data-types.md) | List of resource booking identifiers from the user field of type `resourcebooking` in leads or deals in CRM.

Identifiers can be obtained via:
- universal methods — [crm.item.get](../../crm/universal/crm-item-get.md), [crm.item.list](../../crm/universal/crm-item-list.md)
- methods for leads — [crm.lead.get](../../crm/leads/crm-lead-get.md), [crm.lead.list](../../crm/leads/crm-lead-list.md)
- methods for deals — [crm.deal.get](../../crm/deals/crm-deal-get.md), [crm.deal.list](../../crm/deals/crm-deal-list.md) 

To find out which user fields have the type `resourcebooking`, you can use the [crm.lead.userfield.list](../../crm/leads/userfield/crm-lead-userfield-list.md) method for leads and the [crm.deal.userfield.list](../../crm/deals/user-defined-fields/crm-deal-userfield-list.md) method for deals ||
|#

{% note info " " %}

In the `calendar.resource.booking.list` method, you must use only one of the two required parameters: `resourceTypeIdList` or `resourceIdList`. These parameters cannot be used together.

{% endnote %}

## Code Examples

**Example 1**. Assess resource availability over a period, for instance, to create custom availability views or for use in application logic.

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":{"resourceTypeIdList":[10852,10888,10873,10871,10853],"from":"2024-06-20","to":"2024-08-20"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/calendar.resource.booking.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":{"resourceTypeIdList":[10852,10888,10873,10871,10853],"from":"2024-06-20","to":"2024-08-20"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/calendar.resource.booking.list
    ```

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

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'calendar.resource.booking.list',
        [
            'filter' => [
                'resourceTypeIdList' => [10852, 10888, 10873, 10871, 10853],
                'from' => '2024-06-20',
                'to' => '2024-08-20'
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}


**Example 2**. Select bookings by their identifiers from user fields of the CRM entity.

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":{"resourceIdList":[10,18,17]}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/calendar.resource.booking.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":{"resourceIdList":[10,18,17]},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/calendar.resource.booking.list
    ```

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

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'calendar.resource.booking.list',
        [
            'filter' => [
                'resourceIdList' => [10, 18, 17]
            ]
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
            "ID": "1408",
            "PARENT_ID": "1408",
            "DELETED": "N",
            "CAL_TYPE": "resource",
            "OWNER_ID": "0",
            "NAME": "Booking",
            "DATE_FROM": "20.12.2024 00:00:00",
            "DATE_TO": "21.12.2024 00:00:00",
            "TZ_FROM": "Europe/Riga",
            "TZ_TO": "Europe/Riga",
            "TZ_OFFSET_FROM": "7200",
            "TZ_OFFSET_TO": "7200",
            "DATE_FROM_TS_UTC": "1734652800",
            "DATE_TO_TS_UTC": "1734739200",
            "DT_SKIP_TIME": "Y",
            "DT_LENGTH": 172800,
            "EVENT_TYPE": "#resourcebooking#",
            "CREATED_BY": "1",
            "DATE_CREATE": "18.12.2024 13:55:35",
            "TIMESTAMP_X": "18.12.2024 13:55:35",
            "DESCRIPTION": "Service: some",
            "IS_MEETING": false,
            "MEETING_STATUS": "Y",
            "MEETING_HOST": "0",
            "VERSION": "1",
            "SECTION_ID": "198",
            "DATE_FROM_FORMATTED": "Fri Dec 20 2024",
            "DATE_TO_FORMATTED": "Sat Dec 21 2024",
            "SECT_ID": "198",
            "RESOURCE_BOOKING_ID": "10"
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
[`array`](../../data-types.md) | Array of objects. Each object describes a [booking](#booking) ||
|#

#### Booking Object {#booking}

{% note info " " %}

Technically, a booking is a calendar event. The method retrieves a set of fields similar to those of a calendar event. Some fields remain empty as they are not relevant for bookings. Only relevant or filled fields are listed below.

{% endnote %}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`string`](../../data-types.md) | Booking identifier ||
|| **PARENT_ID**
[`string`](../../data-types.md) | For a booking object, this is always equal to the `ID` field ||
|| **DELETED**
[`string`](../../data-types.md) | Flag indicating whether the booking is deleted. Possible values:
- `Y` — booking is deleted
- `N` — booking is not deleted  ||
|| **CAL_TYPE**
[`string`](../../data-types.md) | Calendar type in which the booking is located ||
|| **OWNER_ID**
[`string`](../../data-types.md) | For a booking object, this is always equal to `'0'` ||
|| **NAME**
[`string`](../../data-types.md) | Booking name ||
|| **DATE_FROM**
[`datetime`](../../data-types.md) | Booking start date ||
|| **DATE_TO**
[`datetime`](../../data-types.md) | Booking end date ||
|| **TZ_FROM**
[`string`](../../data-types.md) | Timezone of the booking start date ||
|| **TZ_TO**
[`string`](../../data-types.md) | Timezone of the booking end date ||
|| **TZ_OFFSET_FROM**
[`string`](../../data-types.md) | Time offset of the booking start time from UTC in seconds ||
|| **TZ_OFFSET_TO**
[`string`](../../data-types.md) | Time offset of the booking end time from UTC in seconds ||
|| **DATE_FROM_TS_UTC**
[`string`](../../data-types.md) | Booking start date and time in UTC in timestamp format ||
|| **DATE_TO_TS_UTC**
[`string`](../../data-types.md) | Booking end date and time in UTC in timestamp format ||
|| **DT_SKIP_TIME**
[`string`](../../data-types.md) | Flag indicating whether the booking lasts all day. Possible values:
- `Y` — all day
- `N` — not all day ||
|| **DT_LENGTH**
[`integer`](../../data-types.md) | Duration of the booking in seconds ||
|| **EVENT_TYPE**
[`string`](../../data-types.md) | Type of booking ||
|| **CREATED_BY**
[`string`](../../data-types.md) | Identifier of the user who created the booking ||
|| **DATE_CREATE**
[`datetime`](../../data-types.md) | Booking creation date ||
|| **TIMESTAMP_X**
[`datetime`](../../data-types.md) | Booking modification date ||
|| **DESCRIPTION**
[`string`](../../data-types.md) | Booking description ||
|| **IS_MEETING**
[`boolean`](../../data-types.md) | For a booking object, this is always `false` ||
|| **MEETING_STATUS**
[`string`](../../data-types.md) | For a booking object, this is always `'Y'` ||
|| **MEETING_HOST**
[`string`](../../data-types.md) | For a booking object, this is always `'0'` ||
|| **VERSION**
[`string`](../../data-types.md) | Version of booking changes ||
|| **SECTION_ID**
[`string`](../../data-types.md) | Identifier of the resource in which the booking is located ||
|| **DATE_FROM_FORMATTED**
[`string`](../../data-types.md) | Formatted booking start date ||
|| **DATE_TO_FORMATTED**
[`string`](../../data-types.md) | Formatted booking end date ||
|| **SECT_ID**
[`string`](../../data-types.md) | Identifier of the resource in which the booking is located ||
|| **RESOURCE_BOOKING_ID**
[`integer`](../../data-types.md) | Booking identifier ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "The required parameter "filter['resourceTypeIdList']" for the method "calendar.resource.booking.list" is not set."
}
```

{% include notitle [Error Handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Error Message** | **Description** ||
|| Empty string | Access denied | Access to the method is prohibited for external users ||
|| Empty string | The required parameter "filter['resourceTypeIdList']" for the method "calendar.resource.booking.list" is not set | Neither of the required parameters: `resourceTypeIdList` or `resourceIdList` was provided. ||
|#

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./calendar-resource-add.md)
- [{#T}](./calendar-resource-update.md)
- [{#T}](./calendar-resource-list.md)
- [{#T}](./calendar-resource-delete.md)