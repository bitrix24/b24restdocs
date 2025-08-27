# Get resource booking.v1.resource.get

> Scope: [`booking`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `booking.v1.resource.get` returns information about a resource by its identifier.

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Resource identifier. 
Can be obtained from the methods [booking.v1.resource.add](./booking-v1-resource-add.md) and [booking.v1.resource.list](./booking-v1-resource-list.md) ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":15}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/booking.v1.resource.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":15,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/booking.v1.resource.get
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'booking.v1.resource.get',
    		{
    			id: 15
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
                'booking.v1.resource.get',
                [
                    'id' => 15,
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
        echo 'Error getting resource: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "booking.v1.resource.get",
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
        'booking.v1.resource.get',
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
        "resource": {
            "confirmationCounterDelay": 10800,
            "confirmationNotificationDelay": 86400,
            "confirmationNotificationRepetitions": null,
            "confirmationNotificationRepetitionsInterval": 10800,
            "delayedCounterDelay": 300,
            "delayedNotificationDelay": 300,
            "description": null,
            "id": 15,
            "infoNotificationDelay": null,
            "isConfirmationNotificationOn": "Y",
            "isDelayedNotificationOn": "N",
            "isFeedbackNotificationOn": "N",
            "isInfoNotificationOn": "Y",
            "isMain": "Y",
            "isReminderNotificationOn": "Y",
            "name": "Name",
            "reminderNotificationDelay": -1,
            "templateTypeConfirmation": "animate",
            "templateTypeDelayed": "animate",
            "templateTypeFeedback": "animate",
            "templateTypeInfo": "inanimate",
            "templateTypeReminder": "base",
            "typeId": 1
        }
    },
    "time": {
        "start": 1746539524.292041,
        "finish": 1746539524.356627,
        "duration": 0.06458592414855957,
        "processing": 0.018703937530517578,
        "date_start": "2025-05-06T16:52:04+02:00",
        "date_finish": "2025-05-06T16:52:04+02:00",
        "operating_reset_at": 1746540124,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response. Contains information about the resource fields. The structure is described [below](#resource) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Resource {#resource}

#|
|| **confirmationCounterDelay**
[`integer`](../../data-types.md) | Time until the record is made in seconds, after which the unconfirmed record counter lights up ||
|| **confirmationDelay**
[`integer`](../../data-types.md) | Time until the record in seconds when the client receives the first message to confirm the record ||
|| **confirmationRepetitions**
[`integer`](../../data-types.md) | Number of messages sent to the client for confirming the record, excluding the first one ||
|| **confirmationRepetitionsInterval**
[`integer`](../../data-types.md) | Interval between confirmation messages, in seconds ||
|| **delayedCounterDelay**
[`integer`](../../data-types.md) | Time in seconds after which to turn on the counter in the calendar ||
|| **delayedDelay**
[`integer`](../../data-types.md) | Time in seconds after which to send a message to the client about the delay ||
|| **description**
[`string`](../../data-types.md) | Description of the resource ||
|| **id**
[`integer`](../../data-types.md) | Resource identifier ||
|| **infoDelay**
[`integer`](../../data-types.md) | Delay in seconds after which the client receives a message about the record ||
|| **isConfirmationNotificationOn**
[`string`](../../data-types.md) | Automatic confirmation of the record. Possible values:
- `Y` — enabled
- `N` — disabled ||
|| **isDelayedNotificationOn**
[`string`](../../data-types.md) | Reminder when the client is late. Possible values:
- `Y` — enabled
- `N` — disabled ||
|| **isFeedbackNotificationOn**
[`string`](../../data-types.md) | Request for feedback. Possible values:
- `Y` — enabled
- `N` — disabled ||
|| **isInfoNotificationOn**
[`string`](../../data-types.md) | Message to the client about the record. Possible values:
- `Y` — enabled
- `N` — disabled ||
|| **isMain**
[`string`](../../data-types.md) | How to display the resource. Possible values:
- `Y` — in the schedule columns
- `N` — when resources intersect ||
|| **isReminderNotificationOn**
[`string`](../../data-types.md) | Reminder about the record. Possible values:
- `Y` — enabled
- `N` — disabled ||
|| **name**
[`string`](../../data-types.md) | Name of the resource ||
|| **reminderDelay**
[`integer`](../../data-types.md) | Time until the record in seconds, after which the client receives a reminder about the record.
Value `-1` — in the morning on the day of the record ||
|| **templateTypeConfirmation**
[`string`](../../data-types.md) | Type of the confirmation message template. Possible values:
- `inanimate` — template for booking equipment and rooms
- `animate` — template for appointments with specialists ||
|| **templateTypeDelayed**
[`string`](../../data-types.md) | Type of the delay message template. Possible values:
- `inanimate` — template for booking equipment and rooms
- `animate` — template for appointments with specialists ||
|| **templateTypeFeedback**
[`string`](../../data-types.md) | Type of the feedback request message template. Possible values:
- `inanimate` — template for booking equipment and rooms
- `animate` — template for appointments with specialists ||
|| **templateTypeInfo**
[`string`](../../data-types.md) | Type of the record message template. Possible values:
- `inanimate` — template for booking equipment and rooms
- `animate` — template for appointments with specialists ||
|| **templateTypeReminder**
[`string`](../../data-types.md) | Type of the reminder message template. Possible values: `base` ||
|| **typeId**
[`integer`](../../data-types.md) | Resource type identifier.

Information about the type can be obtained using the method [booking.v1.resourceType.get](./resource-type/booking-v1-resourcetype-get.md) ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": 1021,
    "error_description": "Resource not found"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `1009` | `Resource not found` | Resource with such `id` not found ||
|| `100` | `Could not find value for parameter {id}` | Required parameter not provided ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./resource-type/index.md)
- [{#T}](./booking-v1-resource-add.md)
- [{#T}](./booking-v1-resource-update.md)
- [{#T}](./booking-v1-resource-delete.md)
- [{#T}](./booking-v1-resource-list.md)