# Get Resource Type booking.v1.resourceType.get

> Scope: [`booking`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `booking.v1.resourceType.get` returns information about the resource type by its identifier.

## Method Parameters

{% include [Footnote about parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of the resource type.
Can be obtained from the methods [booking.v1.resourceType.add](./booking-v1-resourcetype-add.md) and [booking.v1.resourceType.list](./booking-v1-resourcetype-list.md) ||
|#

## Code Examples

{% include [Footnote about examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":15}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/booking.v1.resourceType.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":15,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/booking.v1.resourceType.get
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'booking.v1.resourceType.get',
    		{
    			id: 15
    		}
    	);
    	
    	const result = response.getData().result;
    	console.dir(result);
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
                'booking.v1.resourceType.get',
                [
                    'id' => 15
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
        echo 'Error getting resource type: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "booking.v1.resourceType.get",
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

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'booking.v1.resourceType.get',
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
        "resourceType": {
            "code": "equipment",
            "confirmationCounterDelay": 10800,
            "confirmationNotificationDelay": 86400,
            "confirmationNotificationRepetitions": null,
            "confirmationNotificationRepetitionsInterval": 10800,
            "delayedCounterDelay": 300,
            "delayedNotificationDelay": 300,
            "id": 15,
            "infoNotificationDelay": null,
            "isConfirmationNotificationOn": "Y",
            "isDelayedNotificationOn": "Y",
            "isFeedbackNotificationOn": "N",
            "isReminderNotificationOn": "Y",
            "name": "Equipment",
            "reminderNotificationDelay": -1,
            "templateTypeConfirmation": "inanimate",
            "templateTypeDelayed": "inanimate",
            "templateTypeFeedback": "inanimate",
            "templateTypeReminder": "base"
        }
    },
    "time": {
        "start": 1746539255.201164,
        "finish": 1746539255.280769,
        "duration": 0.0796051025390625,
        "processing": 0.014694929122924805,
        "date_start": "2025-05-06T16:47:35+02:00",
        "date_finish": "2025-05-06T16:47:35+02:00",
        "operating_reset_at": 1746539855,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | The root element of the response. Contains information about the resource type fields. The structure is described [below](#resource) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Resource Type {#resource}

#|
|| **code**
[`string`](../../../data-types.md) | Code of the resource type ||
|| **confirmationCounterDelay**
[`integer`](../../../data-types.md) | Time until the counter for unconfirmed bookings is activated, in seconds ||
|| **confirmationDelay**
[`integer`](../../../data-types.md) | Time until the first message for booking confirmation is sent to the client, in seconds ||
|| **confirmationRepetitions**
[`integer`](../../../data-types.md) | Number of messages sent to the client for booking confirmation, excluding the first one ||
|| **confirmationRepetitionsInterval**
[`integer`](../../../data-types.md) | Interval between booking confirmation messages, in seconds ||
|| **delayedCounterDelay**
[`integer`](../../../data-types.md) | Time in seconds after which the counter in the calendar is activated ||
|| **delayedDelay**
[`integer`](../../../data-types.md) | Time in seconds after which a message about the delay is sent to the client ||
|| **id**
[`integer`](../../../data-types.md) | Identifier of the resource ||
|| **infoDelay**
[`integer`](../../../data-types.md) | Delay in seconds after which a message about the booking is sent to the client ||
|| **isConfirmationNotificationOn**
[`string`](../../../data-types.md) | Automatic booking confirmation. Possible values:
- `Y` — enabled
- `N` — disabled ||
|| **isDelayedNotificationOn**
[`string`](../../../data-types.md) | Reminder when the client is late. Possible values:
- `Y` — enabled
- `N` — disabled ||
|| **isFeedbackNotificationOn**
[`string`](../../../data-types.md) | Feedback request. Possible values:
- `Y` — enabled
- `N` — disabled ||
|| **isReminderNotificationOn**
[`string`](../../../data-types.md) | Booking reminder. Possible values:
- `Y` — enabled
- `N` — disabled ||
|| **name**
[`string`](../../../data-types.md) | Name of the resource ||
|| **reminderDelay**
[`integer`](../../../data-types.md) | Time until the reminder about the booking is sent to the client, in seconds.
Value `-1` means in the morning on the day of the booking ||
|| **templateTypeConfirmation**
[`string`](../../../data-types.md) | Type of the booking confirmation message template. Possible values:
- `inanimate` — template for booking equipment and rooms
- `animate` — template for booking specialists ||
|| **templateTypeDelayed**
[`string`](../../../data-types.md) | Type of the delay message template. Possible values:
- `inanimate` — template for booking equipment and rooms
- `animate` — template for booking specialists ||
|| **templateTypeFeedback**
[`string`](../../../data-types.md) | Type of the feedback request message template. Possible values:
- `inanimate` — template for booking equipment and rooms
- `animate` — template for booking specialists ||
|| **templateTypeReminder**
[`string`](../../../data-types.md) | Type of the reminder message template. Possible values: `base` ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": 1013,
    "error_description": "Resource type not found"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `100` | `Could not find value for parameter {id}` | Required parameter not provided ||
|| `1013` | `Resource type not found` | Resource type with such `id` not found ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](../index.md)
- [{#T}](./booking-v1-resourcetype-add.md)
- [{#T}](./booking-v1-resourcetype-update.md)
- [{#T}](./booking-v1-resourcetype-add.md)
- [{#T}](./booking-v1-resourcetype-list.md)