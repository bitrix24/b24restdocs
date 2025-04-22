# Get Booking Connections booking.v1.booking.externalData.list

> Scope: [`booking`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `booking.v1.booking.externalData.list` returns connections for the specified booking.

## Method Parameters

{% include [Footnote on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **bookingId***
[`integer`](../../../data-types.md) | Booking identifier.
Can be obtained using the methods [booking.v1.booking.add](../booking-v1-booking-add.md) and [booking.v1.booking.list](../booking-v1-booking-list.md) ||
|#

## Code Examples

{% include [Footnote on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        "booking.v1.booking.externalData.list",
        {
            bookingId: 123,
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
    -d '{"bookingId":123}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/booking.v1.booking.externalData.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"bookingId":123,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/booking.v1.booking.externalData.list
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'booking.v1.booking.externalData.list',
        [
            'bookingId' => 123
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
        "externalData": [
            {
                "entityTypeId": "DEAL",
                "moduleId": "crm",
                "value": "1"
            },
            {
                "entityTypeId": "DEAL",
                "moduleId": "crm",
                "value": "2"
            }
        ]
    },
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
[`object`](../../../data-types.md) | Root element of the response. Contains an array of objects with information about connections. The structure is described [below](#externalData) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

#### Connections {#externalData}

#|
|| **Name**
`type` | **Description** ||
|| **entityTypeId**
[`string`](../../../data-types.md) | Object type ID ||
|| **moduleId**
[`string`](../../../data-types.md) | Module identifier ||
|| **value**
[`string`](../../../data-types.md) | Element ID ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": 1021,
    "error_description": "Booking not found"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `1021` | `Booking not found` | Booking with the specified `id` not found ||
|| `100` | `Could not find value for parameter` | Required parameter not provided ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./booking-v1-booking-externaldata-set.md)
- [{#T}](./booking-v1-booking-externaldata-unset.md)