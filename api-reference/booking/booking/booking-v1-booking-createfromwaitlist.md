# Create a Booking from the Waitlist booking.v1.booking.createfromwaitlist

> Scope: [`booking`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `booking.v1.booking.createfromwaitlist` creates a booking based on an entry from the waitlist.

## Method Parameters

{% include [Footnote about parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **waitListId***
[`integer`](../../data-types.md) | Identifier of the entry in the waitlist. 
Can be obtained using the methods [booking.v1.waitlist.add](../waitlist/booking-v1-waitlist-add.md) and [booking.v1.waitlist.list](../waitlist/booking-v1-waitlist-list.md) ||
|| **fields**
[`object`](../../data-types.md) | An object containing field values for creating a booking [(detailed description)](#fields) ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **resourceIds***
[`array`](../../data-types.md#array) | An array of resource identifiers for the booking. 
Resource IDs can be obtained using the method [booking.v1.resource.list](../resource/booking-v1-resource-list.md) ||
|| **name**
[`string`](../../data-types.md) | Name of the booking. 
Default value is an empty string ||
|| **description**
[`string`](../../data-types.md) | Description of the booking. 
Default value is an empty string ||
|| **datePeriod***
[`object`](../../data-types.md#object) | An object containing the booking time [(detailed description)](#datePeriod) ||
|#

### Parameter datePeriod {#datePeriod}

#|
|| **Name**
`type` | **Description** ||
|| **from***
[`object`](../../data-types.md#object) | Start time of the booking in the format `{"timestamp": "1723446900", "timezone": "Europe/Berlin"}`||
|| **to***
[`object`](../../data-types.md#object) | End time of the booking in the format `{"timestamp": "1723447800", "timezone": "Europe/Berlin"}` ||
|#

## Code Examples

{% include [Footnote about examples](../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        "booking.v1.booking.createfromwaitlist",
        {
            waitListId: 10,
            fields: {
                resourceIds: [1, 2, 3],
                datePeriod: {
                    from: {
                        timestamp: 1723446900,
                        timezone: "Europe/Berlin"
                    },
                    to: {
                        timestamp: 1723447800,
                        timezone: "Europe/Berlin"
                    }
                }
            }
        },
        result => {
            if (result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"waitListId":10,"fields":{"resourceIds":[1,2,3],"datePeriod":{"from":{"timestamp":1723446900,"timezone":"Europe/Berlin"},"to":{"timestamp":1723447800,"timezone":"Europe/Berlin"}}}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/booking.v1.booking.createfromwaitlist
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"waitListId":10,"fields":{"resourceIds":[1,2,3],"datePeriod":{"from":{"timestamp":1723446900,"timezone":"Europe/Berlin"},"to":{"timestamp":1723447800,"timezone":"Europe/Berlin"}}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/booking.v1.booking.createfromwaitlist
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'booking.v1.booking.createfromwaitlist',
        [
            'waitListId' => 10,
            'fields' => [
                'resourceIds' => [1, 2, 3],
                'datePeriod' => [
                    'from' => [
                        'timestamp' => 1723446900,
                        'timezone' => 'Europe/Berlin'
                    ],
                    'to' => [
                        'timestamp' => 1723447800,
                        'timezone' => 'Europe/Berlin'
                    ]
                ]
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": 582,
    "time": {
     "start": 1724068028.331234,
        "finish": 1724068028.726591,
        "duration": 0.3953571319580078,
        "processing": 0.13033390045166016,
        "date_start": "2025-01-21T13:47:08+02:00",
        "date_finish": "2025-01-21T13:47:08+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../../data-types.md) | Root element of the response, contains the identifier of the added booking ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": 422,
    "error_description": "Invalid date period"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `1038` | `wait list item not found` | Waitlist with the specified `id` not found, resources unavailable for this time period, empty resource array, or such resources do not exist ||
|| `0` | `Required fields:` | Required parameter not passed within `fields` ||
|| `100` | `Could not find value for parameter` | Required parameter not passed ||
|| `422` | `Invalid date period` | Incorrect time period ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./booking-v1-booking-list.md)
- [{#T}](./booking-v1-booking-update.md)
- [{#T}](./booking-v1-booking-add.md)
- [{#T}](./booking-v1-booking-delete.md)
- [{#T}](./booking-v1-booking-get.md)