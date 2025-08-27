# Update Resource Type booking.v1.resourceType.update

> Scope: [`booking`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `booking.v1.resourceType.update` updates information about an existing resource type.

## Method Parameters

{% include [Footnote about parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of the resource type. 
Can be obtained from the methods [booking.v1.resourceType.add](./booking-v1-resourcetype-add.md) and [booking.v1.resourceType.list](./booking-v1-resourcetype-list.md) ||
|| **fields***
[`object`](../../../data-types.md) | An object containing field values to update the resource type [(detailed description)](#fields) ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **name**
[`string`](../../../data-types.md) | Name of the resource type ||
|| **code**
[`string`](../../../data-types.md) | Code of the resource type ||
|| **isInfoNotificationOn**
[`string`](../../../data-types.md) | Message to the client about the booking. Possible values:
- `Y` — enabled
- `N` — disabled ||
|| **templateTypeInfo**
[`string`](../../../data-types.md) | Type of the booking message template. Possible values:
- `inanimate` — template for booking equipment and rooms
- `animate` — template for booking specialists ||
|| **isConfirmationNotificationOn**
[`string`](../../../data-types.md) | Automatic booking confirmation. Possible values:
- `Y` — enabled
- `N` — disabled ||
|| **templateTypeConfirmation**
[`string`](../../../data-types.md) | Type of the confirmation message template. Possible values:
- `inanimate` — template for booking equipment and rooms
- `animate` — template for booking specialists ||
|| **isReminderNotificationOn**
[`string`](../../../data-types.md) | Reminder about the booking. Possible values:
- `Y` — enabled
- `N` — disabled ||
|| **templateTypeReminder**
[`string`](../../../data-types.md) | Type of the reminder message template. Possible values: `base` ||
|| **isFeedbackNotificationOn**
[`string`](../../../data-types.md) | Request for feedback. Possible values:
- `Y` — enabled
- `N` — disabled ||
|| **templateTypeFeedback**
[`string`](../../../data-types.md) | Type of the feedback request message template. Possible values:
- `inanimate` — template for booking equipment and rooms
- `animate` — template for booking specialists ||
|| **isDelayedNotificationOn**
[`string`](../../../data-types.md) | Reminder when the client is late. Possible values:
- `Y` — enabled
- `N` — disabled ||
|| **templateTypeDelayed**
[`string`](../../../data-types.md) | Type of the delay message template. Possible values:
- `inanimate` — template for booking equipment and rooms
- `animate` — template for booking specialists ||
|| **infoDelay**
[`integer`](../../../data-types.md) | Delay after which the client receives a booking message. Specified in seconds ||
|| **reminderDelay**
[`integer`](../../../data-types.md) | Time before the booking when the client receives a reminder. Specified in seconds ||
|| **delayedDelay**
[`integer`](../../../data-types.md) | Time after which to send a delay message to the client. Specified in seconds ||
|| **delayedCounterDelay**
[`integer`](../../../data-types.md) | Time after which to enable the counter in the calendar. Specified in seconds ||
|| **confirmationDelay**
[`integer`](../../../data-types.md) | Time before the booking when the client receives the first confirmation message. Specified in seconds ||
|| **confirmationRepetitions**
[`integer`](../../../data-types.md) | Number of messages sent to the client for booking confirmation, excluding the first one ||
|| **confirmationRepetitionsInterval**
[`integer`](../../../data-types.md) | Interval between booking confirmation messages. Specified in seconds ||
|| **confirmationCounterDelay**
[`integer`](../../../data-types.md) | Time before the booking after which the counter for unconfirmed bookings lights up. Specified in seconds ||
|#

## Code Examples

{% include [Footnote about examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":10,"fields":{"name":"New Name","code":"Updated Code","isInfoNotificationOn":"Y","templateTypeInfo":"inanimate","isConfirmationNotificationOn":"Y","templateTypeConfirmation":"animate","isReminderNotificationOn":"Y","templateTypeReminder":"base","isFeedbackNotificationOn":"N","templateTypeFeedback":"animate","isDelayedNotificationOn":"N","templateTypeDelayed":"animate","infoDelay":300,"reminderDelay":-1,"delayedDelay":300,"delayedCounterDelay":7200,"confirmationDelay":86400,"confirmationRepetitions":0,"confirmationRepetitionsInterval":0,"confirmationCounterDelay":7200}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/booking.v1.resourceType.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":10,"fields":{"name":"New Name","code":"Updated Code","isInfoNotificationOn":"Y","templateTypeInfo":"inanimate","isConfirmationNotificationOn":"Y","templateTypeConfirmation":"animate","isReminderNotificationOn":"Y","templateTypeReminder":"base","isFeedbackNotificationOn":"N","templateTypeFeedback":"animate","isDelayedNotificationOn":"N","templateTypeDelayed":"animate","infoDelay":300,"reminderDelay":-1,"delayedDelay":300,"delayedCounterDelay":7200,"confirmationDelay":86400,"confirmationRepetitions":0,"confirmationRepetitionsInterval":0,"confirmationCounterDelay":7200},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/booking.v1.resourceType.update
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"booking.v1.resourceType.update",
    		{
    			id: 10,
    			fields: {
    				name: "New Name",
    				code: "Updated Code",
    				isInfoNotificationOn: "Y",
    				templateTypeInfo: "inanimate",
    				isConfirmationNotificationOn: "Y",
    				templateTypeConfirmation: "animate",
    				isReminderNotificationOn: "Y",
    				templateTypeReminder: "base",
    				isFeedbackNotificationOn: "N",
    				templateTypeFeedback: "animate",
    				isDelayedNotificationOn: "N",
    				templateTypeDelayed: "animate",
    				infoDelay: 300,
    				reminderDelay: -1,
    				delayedDelay: 300,
    				delayedCounterDelay: 7200,
    				confirmationDelay: 86400,
    				confirmationRepetitions: 0,
    				confirmationRepetitionsInterval: 0,
    				confirmationCounterDelay: 7200
    			}
    		}
    	);
    	
    	const result = response.getData().result;
    	if (result.error())
    		console.error(result.error());
    	else
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
                'booking.v1.resourceType.update',
                [
                    'id' => 10,
                    'fields' => [
                        'name' => 'New Name',
                        'code' => 'Updated Code',
                        'isInfoNotificationOn' => 'Y',
                        'templateTypeInfo' => 'inanimate',
                        'isConfirmationNotificationOn' => 'Y',
                        'templateTypeConfirmation' => 'animate',
                        'isReminderNotificationOn' => 'Y',
                        'templateTypeReminder' => 'base',
                        'isFeedbackNotificationOn' => 'N',
                        'templateTypeFeedback' => 'animate',
                        'isDelayedNotificationOn' => 'N',
                        'templateTypeDelayed' => 'animate',
                        'infoDelay' => 300,
                        'reminderDelay' => -1,
                        'delayedDelay' => 300,
                        'delayedCounterDelay' => 7200,
                        'confirmationDelay' => 86400,
                        'confirmationRepetitions' => 0,
                        'confirmationRepetitionsInterval' => 0,
                        'confirmationCounterDelay' => 7200,
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your data processing logic
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating resource type: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "booking.v1.resourceType.update",
        {
            id: 10,
            fields: {
                name: "New Name",
                code: "Updated Code",
                isInfoNotificationOn: "Y",
                templateTypeInfo: "inanimate",
                isConfirmationNotificationOn: "Y",
                templateTypeConfirmation: "animate",
                isReminderNotificationOn: "Y",
                templateTypeReminder: "base",
                isFeedbackNotificationOn: "N",
                templateTypeFeedback: "animate",
                isDelayedNotificationOn: "N",
                templateTypeDelayed: "animate",
                infoDelay: 300,
                reminderDelay: -1,
                delayedDelay: 300,
                delayedCounterDelay: 7200,
                confirmationDelay: 86400,
                confirmationRepetitions: 0,
                confirmationRepetitionsInterval: 0,
                confirmationCounterDelay: 7200
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

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'booking.v1.resourceType.update',
        [
            'id' => 10,
            'fields' => [
                'name' => 'New Name',
                'code' => 'Updated Code',
                'isInfoNotificationOn' => 'Y',
                'templateTypeInfo' => 'inanimate',
                'isConfirmationNotificationOn' => 'Y',
                'templateTypeConfirmation' => 'animate',
                'isReminderNotificationOn' => 'Y',
                'templateTypeReminder' => 'base',
                'isFeedbackNotificationOn' => 'N',
                'templateTypeFeedback' => 'animate',
                'isDelayedNotificationOn' => 'N',
                'templateTypeDelayed' => 'animate',
                'infoDelay' => 300,
                'reminderDelay' => -1,
                'delayedDelay' => 300,
                'delayedCounterDelay' => 7200,
                'confirmationDelay' => 86400,
                'confirmationRepetitions' => 0,
                'confirmationRepetitionsInterval' => 0,
                'confirmationCounterDelay' => 7200
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
[`boolean`](../../../data-types.md) | Root element of the response, contains `true` in case of success ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": 1007,
    "error_description": "Resource type not found"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `1007` | `Resource type not found` | A non-existent `id` of the resource type was specified ||
|| `100` | `Could not find value for parameter` | A required parameter was not provided ||
|| `422` | `Invalid value of the field` | Incorrect field value ||
|| `1011` | `Resource type with code already exists` | A resource type with this `code` already exists ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](../index.md)
- [{#T}](./booking-v1-resourcetype-get.md)
- [{#T}](./booking-v1-resourcetype-add.md)
- [{#T}](./booking-v1-resourcetype-delete.md)
- [{#T}](./booking-v1-resourcetype-list.md)