# Get Booking Information booking.v1.booking.get

> Scope: [`booking`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `booking.v1.booking.get` returns information about a booking by its identifier.

## Method Parameters

{% include [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Booking identifier ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        "booking.v1.booking.get",
        {
            id: 15
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
    -d '{"id":15}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/booking.v1.booking.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":15,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/booking.v1.booking.get
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'booking.v1.booking.get',
        [
            'id' => 15
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
"result": {
    "booking": {
     "datePeriod": {
        "from": {
         "timestamp": 1723446900,
         "timezone": "Europe/Berlin"
        },
        "to": {
         "timestamp": 1723447800,
         "timezone": "Europe/Berlin"
        }
     },
     "description": null,
     "id": 15,
     "name": "booking 1",
     "resourceIds": [
        1,
        2
     ]
    }
},
"time": {
    "start": 1741002780.099604,
    "finish": 1741002780.381558,
    "duration": 0.2819540500640869,
    "processing": 0.14896297454833984,
    "date_start": "2025-03-03T11:53:00+00:00",
    "date_finish": "2025-03-03T11:53:00+00:00",
    "operating": 0.14890098571777344
}
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response. Contains information about the booking fields. The structure is described [below](#booking) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Booking {#booking}

#|
|| **datePeriod**
[`object`](../../data-types.md) | Time period of the booking. Contains `from` and `to` fields with information about the start and end times of the booking ||
|| **description**
[`string`](../../data-types.md) | Description of the booking. Can be `null` ||
|| **id**
[`integer`](../../data-types.md) | Booking identifier ||
|| **name**
[`string`](../../data-types.md) | Name of the booking ||
|| **resourceIds**
[`array`](../../data-types.md) | Array of resource identifiers associated with the booking. Resource descriptions can be obtained using the method [booking.v1.resource.get](../resource/booking-v1-resource-get.md) ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": 1021,
    "error_description": "Booking not found"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `1021` | `Booking not found` | Booking with such `id` not found ||
|| `100` | `Could not find value for parameter ` | Required parameter not provided ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./booking-v1-booking-createfromwaitlist.md)
- [{#T}](./booking-v1-booking-update.md)
- [{#T}](./booking-v1-booking-add.md)
- [{#T}](./booking-v1-booking-list.md)
- [{#T}](./booking-v1-booking-delete.md)