# Get Slot Settings for Resource booking.v1.resource.slots.list

> Scope: [`booking`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `booking.v1.resource.slots.list` returns the configuration of time slots for the specified resource.

## Method Parameters

{% include [Footnote about parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **resourceID***
[`integer`](../../../data-types.md) | Resource identifier.
Can be obtained using the methods [booking.v1.resource.add](../booking-v1-resource-add.md) and [booking.v1.resource.list](../booking-v1-resource-list.md) ||
|#

## Code Examples

{% include [Footnote about examples](../../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        "booking.v1.resource.slots.list",
        {
            resourceId: 257,
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
    -d '{"resourceId":257}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/booking.v1.resource.slots.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"resourceId":257,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/booking.v1.resource.slots.list
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'booking.v1.resource.slots.list',
        [
            'resourceId' => 257
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
    "result": {
        "slots": [
            {
                "from": 100,
                "id": 171,
                "slotSize": 1,
                "timezone": "Europe/Berlin",
                "to": 300,
                "weekDays": [
                    "Mon",
                    "Tue"
                ]
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
[`object`](../../../data-types.md) | Root element of the response. 

Contains an array of objects with information about the slots. The structure is described [below](#slots) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Slot {#slots}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Identifier of the slot settings ||
|| **from**
[`integer`](../../../data-types.md) | Time from which slot booking is available during the day. Value in the range from 0 to 1440. For example, `540` means booking is available from 9:00 ||
|| **to**
[`integer`](../../../data-types.md) | End time of the slot in minutes. Value in the range from 0 to 1440, greater than or equal to the `from` value. For example, `1080` means booking is available until 18:00 ||
|| **timezone**
[`string`](../../../data-types.md) | Timezone relative to which the slot time is set ||
|| **weekDays**
[`array`](../../../data-types.md) | Array of available weekdays for the slot. Possible values: 
- `"Mon"` — Monday
- `"Tue"` — Tuesday
- `"Wed"` — Wednesday
- `"Thu"` — Thursday
- `"Fri"` — Friday
- `"Sat"` — Saturday
- `"Sun"` — Sunday ||
|| **slotSize**
[`integer`](../../../data-types.md) | Duration of the slot in minutes ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": 1009,
    "error_description": "Resource not found"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `1009` | `Resource not found` | Resource with the specified `id` not found ||
|| `100` | `Could not find value for parameter` | Required parameter not provided ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./booking-v1-resource-slots-set.md)
- [{#T}](./booking-v1-resource-slots-unset.md)