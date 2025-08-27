# Set Slots for Resource booking.v1.resource.slots.set

> Scope: [`booking`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `booking.v1.resource.slots.set` allows you to set time slots for the specified resource.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **resourceId***
[`integer`](../../../data-types.md) | Resource identifier. 
Can be obtained using the methods [booking.v1.resource.add](../booking-v1-resource-add.md) and [booking.v1.resource.list](../booking-v1-resource-list.md) ||
|| **slots***
[`array`](../../../data-types.md) | An array of objects containing field values for setting time slots [(detailed description)](#slots) ||
|#

### Parameter slots {#slots}

#|
|| **Name**
`type` | **Description** ||
|| **from***
[`integer`](../../../data-types.md) | The time from which slot booking is available during the day. Value in the range from 0 to 1440. For example, `540` means booking is available from 9:00 AM ||
|| **to***
[`integer`](../../../data-types.md) | The end time of the slot in minutes. Value in the range from 0 to 1440, greater than or equal to the value of `from`. For example, `1080` means booking is available until 6:00 PM ||
|| **timezone***
[`string`](../../../data-types.md) | The time zone relative to which the slot time is set ||
|| **weekDays***
[`array`](../../../data-types.md) | An array of available weekdays for the slot. Possible values: 
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

{% include [Note on examples](../../../../_includes/examples.md) %}

Example of setting time slots for a resource:
- availability from Monday to Friday from 9:00 AM to 6:00 PM in the GMT+2 time zone
- slot duration of 30 minutes

{% list tabs %}

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

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'booking.v1.resource.slots.set',
    		{
    			resourceId: 10,
    			slots: [
    				{
    					from: 540,
    					to: 1080,
    					timezone: 'Europe/Berlin',
    					weekDays: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri'],
    					slotSize: 30
    				}
    			]
    		}
    	);
    	
    	const result = response.getData().result;
    	if (result.error())
    	{
    		console.error(result.error());
    	}
    	else
    	{
    		console.dir(result);
    	}
    }
    catch( error )
    {
    	console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'booking.v1.resource.slots.set',
                [
                    'resourceId' => 10,
                    'slots'      => [
                        [
                            'from'     => 540,
                            'to'       => 1080,
                            'timezone' => 'Europe/Berlin',
                            'weekDays' => ['Mon', 'Tue', 'Wed', 'Thu', 'Fri'],
                            'slotSize' => 30
                        ]
                    ]
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
        echo 'Error setting resource slots: ' . $e->getMessage();
    }
    ```

- BX24.js

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

- PHP CRest

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
[`boolean`](../../../data-types.md) | Root element of the response, contains `true` in case of success  ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

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