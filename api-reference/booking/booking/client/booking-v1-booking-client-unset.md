# Remove clients from booking booking.v1.booking.client.unset

> Scope: [`booking`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `booking.v1.booking.client.unset` removes clients for the specified booking.

## Method Parameters

{% include [Footnote about parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **bookingId***
[`integer`](../../../data-types.md) | Booking identifier.
Can be obtained using the methods [booking.v1.booking.add](../booking-v1-booking-add.md) and [booking.v1.booking.list](../booking-v1-booking-list.md) ||
|#

## Code Examples

{% include [Footnote about examples](../../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        "booking.v1.booking.client.unset",
        {
            bookingId: 14,
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
    -d '{"bookingId":14,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/booking.v1.booking.client.unset
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"bookingId":14}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/booking.v1.booking.client.unset
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'booking.v1.booking.client.unset',
        [
            'bookingId' => 14,
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
    "result": true,
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
[`boolean`](../../../data-types.md) | Root element of the response, contains `true` in case of success ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
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

- [{#T}](./booking-v1-booking-client-set.md)
- [{#T}](./booking-v1-booking-client-list.md)