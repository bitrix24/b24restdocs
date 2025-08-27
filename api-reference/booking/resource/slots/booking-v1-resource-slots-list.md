# Get Slot Settings for Resource booking.v1.resource.slots.list

> Scope: [`booking`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `booking.v1.resource.slots.list` returns the configuration of time slots for the specified resource.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **resourceID***
[`integer`](../../../data-types.md) | Resource identifier.
Can be obtained using the methods [booking.v1.resource.add](../booking-v1-resource-add.md) and [booking.v1.resource.list](../booking-v1-resource-list.md) ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

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

- JS

    ```js
    // callListMethod is recommended when you need to retrieve the entire set of list data and the volume of records is relatively small (up to about 1000 items). The method loads all data at once, which can lead to high memory load when working with large volumes.
    
    try {
      const response = await $b24.callListMethod(
        'booking.v1.resource.slots.list',
        { resourceId: 257 },
        (progress) => { console.log('Progress:', progress) }
      )
      const items = response.getData() || []
      for (const entity of items) { console.log('Entity:', entity) }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // fetchListMethod is preferable when working with large datasets. The method implements iterative selection using a generator, allowing data to be processed in parts and efficiently using memory.
    
    try {
      const generator = $b24.fetchListMethod('booking.v1.resource.slots.list', { resourceId: 257 }, 'ID')
      for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity) }
      }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // callMethod provides manual control over the pagination process through the start parameter. Suitable for scenarios where precise control over request batches is required. However, with large volumes of data, it may be less efficient compared to fetchListMethod.
    
    try {
      const response = await $b24.callMethod('booking.v1.resource.slots.list', { resourceId: 257 }, 0)
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
                'booking.v1.resource.slots.list',
                [
                    'resourceId' => 257,
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error listing resource slots: ' . $e->getMessage();
    }
    ```

- BX24.js

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

- PHP CRest

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
     "date_start": "2025-01-21T13:47:08+01:00",
     "date_finish": "2025-01-21T13:47:08+01:00",
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
[`integer`](../../../data-types.md) | Time from which slots are available for booking during the day. Value in the range from 0 to 1440. For example, `540` means slots are available for booking from 9:00 ||
|| **to**
[`integer`](../../../data-types.md) | End time of the slot in minutes. Value in the range from 0 to 1440, greater than or equal to the value of `from`. For example, `1080` means slots are available for booking until 18:00 ||
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