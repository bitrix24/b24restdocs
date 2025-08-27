# Get Resource Bookings by Filter calendar.resource.booking.list

> Scope: [`calendar`](../../scopes/permissions.md)
>
> Who can execute the method: any user

This method retrieves resource bookings based on a filter.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **filter***
[`object`](../../data-types.md) | Filter fields ||
|#

### Filter Parameter

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **resourceTypeIdList***
[`array`](../../data-types.md) | List of resource identifiers.

Identifiers can be obtained using the method [calendar.resource.list](./calendar-resource-list.md) ||
|| **from**
[`date`](../../data-types.md) | Start date of the period ||
|| **to**
[`date`](../../data-types.md) | End date of the period ||
|| **resourceIdList***
[`array`](../../data-types.md) | List of resource booking identifiers from the custom field of type `resourcebooking` in leads or deals in CRM.

Identifiers can be obtained via:
- universal methods — [crm.item.get](../../crm/universal/crm-item-get.md), [crm.item.list](../../crm/universal/crm-item-list.md)
- methods for leads — [crm.lead.get](../../crm/leads/crm-lead-get.md), [crm.lead.list](../../crm/leads/crm-lead-list.md)
- methods for deals — [crm.deal.get](../../crm/deals/crm-deal-get.md), [crm.deal.list](../../crm/deals/crm-deal-list.md) 

To find out which custom fields have the type `resourcebooking`, you can use the method [crm.lead.userfield.list](../../crm/leads/userfield/crm-lead-userfield-list.md) for leads and the method [crm.deal.userfield.list](../../crm/deals/user-defined-fields/crm-deal-userfield-list.md) for deals ||
|#

{% note info " " %}

In the method `calendar.resource.booking.list`, you must use only one of the two required parameters: `resourceTypeIdList` or `resourceIdList`. Both parameters cannot be used together.

{% endnote %}

## Code Examples

**Example 1**. Assess resource availability over a period, for instance, to create custom views of availability or for use in application logic.

{% include [Note on examples](../../../_includes/examples.md) %}

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
    // callListMethod is recommended when you need to retrieve the entire set of list data and the volume of records is relatively small (up to about 1000 items). The method loads all data at once, which can lead to high memory load when working with large volumes.
    
    try {
      const response = await $b24.callListMethod(
        'calendar.resource.booking.list',
        {
          filter: {
            resourceTypeIdList: [10852, 10888, 10873, 10871, 10853],
            from: '2024-06-20',
            to: '2024-08-20',
          }
        },
        (progress) => { console.log('Progress:', progress) }
      );
      const items = response.getData() || [];
      for (const entity of items) { console.log('Entity:', entity); }
    } catch (error) {
      console.error('Request failed', error);
    }
    
    // fetchListMethod is preferable when working with large datasets. The method implements iterative selection using a generator, allowing data to be processed in parts and efficiently using memory.
    
    try {
      const generator = $b24.fetchListMethod('calendar.resource.booking.list', {
        filter: {
          resourceTypeIdList: [10852, 10888, 10873, 10871, 10853],
          from: '2024-06-20',
          to: '2024-08-20',
        }
      }, 'ID');
      for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity); }
      }
    } catch (error) {
      console.error('Request failed', error);
    }
    
    // callMethod provides manual control over the pagination process through the start parameter. Suitable for scenarios where precise control over request batches is required. However, with large volumes of data, it may be less efficient compared to fetchListMethod.
    
    try {
      const response = await $b24.callMethod('calendar.resource.booking.list', {
        filter: {
          resourceTypeIdList: [10852, 10888, 10873, 10871, 10853],
          from: '2024-06-20',
          to: '2024-08-20',
        }
      }, 0);
      const result = response.getData().result || [];
      for (const entity of result) { console.log('Entity:', entity); }
    } catch (error) {
      console.error('Request failed', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'calendar.resource.booking.list',
                [
                    'filter' => [
                        'resourceTypeIdList' => [10852, 10888, 10873, 10871, 10853],
                        'from'              => '2024-06-20',
                        'to'                => '2024-08-20',
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your logic for processing data
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching resource booking list: ' . $e->getMessage();
    }
    ```

- BX24.js

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

- PHP CRest

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


**Example 2**. Select bookings by their identifiers from custom fields of the CRM entity.

{% include [Note on examples](../../../_includes/examples.md) %}

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
    // callListMethod is recommended when you need to retrieve the entire set of list data and the volume of records is relatively small (up to about 1000 items). The method loads all data at once, which can lead to high memory load when working with large volumes.
    
    try {
      const response = await $b24.callListMethod(
        'calendar.resource.booking.list',
        {
          filter: {
            resourceIdList: [10, 18, 17]
          }
        },
        (progress) => { console.log('Progress:', progress) }
      )
      const items = response.getData() || []
      for (const entity of items) { console.log('Entity:', entity) }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // fetchListMethod is preferable when working with large datasets. The method implements iterative selection using a generator, allowing data to be processed in parts and efficiently using memory.
    
    try {
      const generator = $b24.fetchListMethod('calendar.resource.booking.list', {
        filter: {
          resourceIdList: [10, 18, 17]
        }
      }, 'ID')
      for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity) }
      }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // callMethod provides manual control over the pagination process through the start parameter. Suitable for scenarios where precise control over request batches is required. However, with large volumes of data, it may be less efficient compared to fetchListMethod.
    
    try {
      const response = await $b24.callMethod('calendar.resource.booking.list', {
        filter: {
          resourceIdList: [10, 18, 17]
        }
      }, 0)
      const result = response.getData().result || []
      for (const entity of result) { console.log('Entity:', entity) }
    } catch (error) {
      console.error('Request failed', error)
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'calendar.resource.booking.list',
                [
                    'filter' => [
                        'resourceIdList' => [10, 18, 17]
                    ]
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your logic for processing data
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching resource booking list: ' . $e->getMessage();
    }
    ```

- BX24.js

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

- PHP CRest

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

Technically, a booking is a calendar event. The method retrieves a set of fields similar to those of a calendar event. Some fields remain empty as they are not relevant for bookings. Below are only the relevant or filled fields.

{% endnote %}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`string`](../../data-types.md) | Booking identifier ||
|| **PARENT_ID**
[`string`](../../data-types.md) | For a booking object, always equal to the `ID` field ||
|| **DELETED**
[`string`](../../data-types.md) | Flag indicating whether the booking is deleted. Possible values:
- `Y` — booking deleted
- `N` — booking not deleted  ||
|| **CAL_TYPE**
[`string`](../../data-types.md) | Type of calendar in which the booking is located ||
|| **OWNER_ID**
[`string`](../../data-types.md) | For a booking object, always equals `'0'` ||
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
[`datetime`](../../data-types.md) | Creation date of the booking ||
|| **TIMESTAMP_X**
[`datetime`](../../data-types.md) | Date of modification of the booking ||
|| **DESCRIPTION**
[`string`](../../data-types.md) | Description of the booking ||
|| **IS_MEETING**
[`boolean`](../../data-types.md) | For a booking object, always `false` ||
|| **MEETING_STATUS**
[`string`](../../data-types.md) | For a booking object, always `'Y'` ||
|| **MEETING_HOST**
[`string`](../../data-types.md) | For a booking object, always `'0'` ||
|| **VERSION**
[`string`](../../data-types.md) | Version of booking changes ||
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

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "The required parameter "filter['resourceTypeIdList']" is not set for the method "calendar.resource.booking.list""
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Error Message** | **Description** ||
|| Empty string | Access denied | Access to the method is prohibited for external users ||
|| Empty string | The required parameter "filter['resourceTypeIdList']" is not set for the method "calendar.resource.booking.list" | Neither of the required parameters: `resourceTypeIdList` or `resourceIdList` was provided. ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./calendar-resource-add.md)
- [{#T}](./calendar-resource-update.md)
- [{#T}](./calendar-resource-list.md)
- [{#T}](./calendar-resource-delete.md)