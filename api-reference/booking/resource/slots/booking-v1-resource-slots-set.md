# Set Slots for Resource booking.v1.resource.slots.set

> Scope: [`booking`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `booking.v1.resource.slots.set` allows you to set time slots for the specified resource.

## Method Parameters

{% include [Footnote about parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **resourceId***  
[`integer`](../../../data-types.md) | Identifier of the resource.  
Can be obtained using the methods [booking.v1.resource.add](../booking-v1-resource-add.md) and [booking.v1.resource.list](../booking-v1-resource-list.md) ||
|| **slots***  
[`array`](../../../data-types.md) | Array of objects containing field values for setting time slots [(detailed description)](#slots) ||
|#

### Parameter slots {#slots}

#|
|| **Name**
`type` | **Description** ||
|| **from***  
[`integer`](../../../data-types.md) | Time from which the slots are available for booking during the day. Value in the range from 0 to 1440. For example, `540` means slots are available from 9:00 AM ||
|| **to***  
[`integer`](../../../data-types.md) | End time of the slot in minutes. Value in the range from 0 to 1440, greater than or equal to the `from` value. For example, `1080` means slots are available until 6:00 PM ||
|| **timezone***  
[`string`](../../../data-types.md) | Timezone relative to which the slot time is set ||
|| **weekDays***  
[`array`](../../../data-types.md) | Array of available weekdays for the slot. Possible values: 
- `"Mon"` — Monday
- `"Tue"` — Tuesday
- `"Wed"` — Wednesday
- `"Thu"` — Thursday
- `"Fri"` — Friday
- `"Sat"` — Saturday
- `"Sun"` — Sunday ||
|| **slotSize***  
[`integer`](../../../data-types.md) | Duration of the slot in minutes ||
|#

## Code Examples

{% include [Footnote about examples](../../../../_includes/examples.md) %}

Example of setting time slots for a resource:
- availability from Monday to Friday from 9:00 AM to 6:00 PM in the GMT+2 timezone
- slot duration of 30 minutes

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        "booking.v1.resource.slots.set",
        {
            resourceId: 10,
            slots: [
                {
                    from: 540,
                    to: 1080,
                    timezone: "Europe/Berlin",
                    weekDays: ["Mon", "Tue", "Wed", "Thu", "Fri"],
                    slotSize: 30
                }
            ]
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
    -d '{"resourceId":10,"slots":[{"from":540,"to":1080,"timezone":"Europe/Berlin","weekDays":["Mon","Tue","Wed","Thu","Fri"],"slotSize":30}],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/booking.v1.resource.slots.set
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"resourceId":10,"slots":[{"from":540,"to":1080,"timezone":"Europe/Berlin","weekDays":["Mon","Tue","Wed","Thu","Fri"],"slotSize":30}]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/booking.v1.resource.slots.set
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'booking.v1.resource.slots.set',
        [
            'resourceId' => 10,
            'slots' => [
                [
                    'from' => 540,
                    'to' => 1080,
                    'timezone' => 'Europe/Berlin',
                    'weekDays' => ['Mon', 'Tue', 'Wed', 'Thu', 'Fri'],
                    'slotSize' => 30
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

HTTP Status: **200**

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
[`boolean`](../../../data-types.md) | Root element of the response, contains `true` in case of success  ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
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
|| `0` | `Required fields:` | Required parameter not provided within `slots` ||
|| `100` | `Could not find value for parameter` | Required parameter not provided ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./booking-v1-resource-slots-unset.md)
- [{#T}](./booking-v1-resource-slots-list.md)