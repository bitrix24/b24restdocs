# Add a New Resource Type booking.v1.resourceType.add

> Scope: [`booking`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `booking.v1.resourceType.add` adds a new resource type.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../../data-types.md) | An object containing field values for creating a resource type [(detailed description)](#fields) ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../../data-types.md) | The name of the resource type ||
|| **code***
[`string`](../../../data-types.md) | A unique code for the resource type ||
|| **isInfoNotificationOn**
[`string`](../../../data-types.md) | Message to the client about the booking. Possible values:
- `Y` — enabled
- `N` — disabled

Default is `Y` ||
|| **templateTypeInfo**
[`string`](../../../data-types.md) | The type of message template for the booking. Possible values:
- `inanimate` — template for booking equipment and rooms
- `animate` — template for booking specialists

Default is `inanimate` ||
|| **isConfirmationNotificationOn**
[`string`](../../../data-types.md) | Automatic confirmation of the booking. Possible values:
- `Y` — enabled
- `N` — disabled

Default is `Y` ||
|| **templateTypeConfirmation**
[`string`](../../../data-types.md) | The type of message template for booking confirmation. Possible values:
- `inanimate` — template for booking equipment and rooms
- `animate` — template for booking specialists

Default is `inanimate` ||
|| **isReminderNotificationOn**
[`string`](../../../data-types.md) | Reminder about the booking. Possible values:
- `Y` — enabled
- `N` — disabled

Default is `Y` ||
|| **templateTypeReminder**
[`string`](../../../data-types.md) | The type of message template for reminders. Possible values: `base` ||
|| **isFeedbackNotificationOn**
[`string`](../../../data-types.md) | Request for feedback. Possible values:
- `Y` — enabled
- `N` — disabled

Default is `Y` ||
|| **templateTypeFeedback**
[`string`](../../../data-types.md) | The type of message template for feedback requests. Possible values:
- `inanimate` — template for booking equipment and rooms
- `animate` — template for booking specialists

Default is `inanimate` ||
|| **isDelayedNotificationOn**
[`string`](../../../data-types.md) | Reminder when the client is late. Possible values:
- `Y` — enabled
- `N` — disabled

Default is `Y` ||
|| **templateTypeDelayed**
[`string`](../../../data-types.md) | The type of message template for delays. Possible values:
- `inanimate` — template for booking equipment and rooms
- `animate` — template for booking specialists

Default is `inanimate` ||
|| **infoDelay**
[`integer`](../../../data-types.md) | Delay after which the client receives a booking message. Specified in seconds.

Default is 300 ||
|| **reminderDelay**
[`integer`](../../../data-types.md) | Time before the booking when the client receives a reminder. Specified in seconds.

Default is -1, in the morning on the day of the booking ||
|| **delayedDelay**
[`integer`](../../../data-types.md) | Time after which to send a message about the delay to the client. Specified in seconds.

Default is 300 ||
|| **delayedCounterDelay**
[`integer`](../../../data-types.md) | Time after which to enable the counter in the calendar. Specified in seconds.

Default is 7200 ||
|| **confirmationDelay**
[`integer`](../../../data-types.md) | Time before the booking when the client receives the first confirmation message. Specified in seconds.

Default is 86400 ||
|| **confirmationRepetitions**
[`integer`](../../../data-types.md) | The number of messages sent to the client for booking confirmation, excluding the first.

Default is 0 ||
|| **confirmationRepetitionsInterval**
[`integer`](../../../data-types.md) | Interval between confirmation messages. Specified in seconds.

Default is 0 ||
|| **confirmationCounterDelay**
[`integer`](../../../data-types.md) | Time before the booking after which the counter for unconfirmed bookings lights up. Specified in seconds.

Default is 7200 ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"name":"Name","code":"code","isInfoNotificationOn":"Y","templateTypeInfo":"inanimate","isConfirmationNotificationOn":"Y","templateTypeConfirmation":"animate","isReminderNotificationOn":"N","templateTypeReminder":"base","isFeedbackNotificationOn":"Y","templateTypeFeedback":"inanimate","isDelayedNotificationOn":"Y","templateTypeDelayed":"inanimate","infoDelay":300,"reminderDelay":-1,"delayedDelay":300,"delayedCounterDelay":7200,"confirmationDelay":86400,"confirmationRepetitions":0,"confirmationRepetitionsInterval":0,"confirmationCounterDelay":7200}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/booking.v1.resourceType.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"name":"Name","code":"code","isInfoNotificationOn":"Y","templateTypeInfo":"inanimate","isConfirmationNotificationOn":"Y","templateTypeConfirmation":"animate","isReminderNotificationOn":"N","templateTypeReminder":"base","isFeedbackNotificationOn":"Y","templateTypeFeedback":"inanimate","isDelayedNotificationOn":"Y","templateTypeDelayed":"inanimate","infoDelay":300,"reminderDelay":-1,"delayedDelay":300,"delayedCounterDelay":7200,"confirmationDelay":86400,"confirmationRepetitions":0,"confirmationRepetitionsInterval":0,"confirmationCounterDelay":7200},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/booking.v1.resourceType.add
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"booking.v1.resourceType.add",
    		{
    			fields: {
    				name: "Name",
    				code: "code",
    				isInfoNotificationOn: "Y",
    				templateTypeInfo: "inanimate",
    				isConfirmationNotificationOn: "Y",
    				templateTypeConfirmation: "animate",
    				isReminderNotificationOn: "N",
    				templateTypeReminder: "base",
    				isFeedbackNotificationOn: "Y",
    				templateTypeFeedback: "inanimate",
    				isDelayedNotificationOn: "Y",
    				templateTypeDelayed: "inanimate",
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
    	console.dir(result);
    }
    catch( error )
    {
    	console.error(error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'booking.v1.resourceType.add',
                [
                    'fields' => [
                        'name'                          => 'Name',
                        'code'                          => 'code',
                        'isInfoNotificationOn'          => 'Y',
                        'templateTypeInfo'              => 'inanimate',
                        'isConfirmationNotificationOn'  => 'Y',
                        'templateTypeConfirmation'      => 'animate',
                        'isReminderNotificationOn'      => 'N',
                        'templateTypeReminder'          => 'base',
                        'isFeedbackNotificationOn'      => 'Y',
                        'templateTypeFeedback'          => 'inanimate',
                        'isDelayedNotificationOn'       => 'Y',
                        'templateTypeDelayed'           => 'inanimate',
                        'infoDelay'                     => 300,
                        'reminderDelay'                 => -1,
                        'delayedDelay'                  => 300,
                        'delayedCounterDelay'           => 7200,
                        'confirmationDelay'             => 86400,
                        'confirmationRepetitions'       => 0,
                        'confirmationRepetitionsInterval' => 0,
                        'confirmationCounterDelay'      => 7200,
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your logic for processing data
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding resource type: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "booking.v1.resourceType.add",
        {
            fields: {
                name: "Name",
                code: "code",
                isInfoNotificationOn: "Y",
                templateTypeInfo: "inanimate",
                isConfirmationNotificationOn: "Y",
                templateTypeConfirmation: "animate",
                isReminderNotificationOn: "N",
                templateTypeReminder: "base",
                isFeedbackNotificationOn: "Y",
                templateTypeFeedback: "inanimate",
                isDelayedNotificationOn: "Y",
                templateTypeDelayed: "inanimate",
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
        'booking.v1.resourceType.add',
        [
            'fields' => [
                'name' => 'Name',
                'code' => 'code',
                'isInfoNotificationOn' => 'Y',
                'templateTypeInfo' => 'inanimate',
                'isConfirmationNotificationOn' => 'Y',
                'templateTypeConfirmation' => 'animate',
                'isReminderNotificationOn' => 'N',
                'templateTypeReminder' => 'base',
                'isFeedbackNotificationOn' => 'Y',
                'templateTypeFeedback' => 'inanimate',
                'isDelayedNotificationOn' => 'Y',
                'templateTypeDelayed' => 'inanimate',
                'infoDelay' => 300,
                'reminderDelay' => -1,
                'delayedDelay' => 300,
                'delayedCounterDelay' => 7200,
                'confirmationDelay' => 86400,
                'confirmationRepetitions' => 0,
                'confirmationRepetitionsInterval' => 0,
                'confirmationCounterDelay' => 7200,
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
    "result": {
        "id": 17
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
[`object`](../../../data-types.md) | The root element of the response, contains the identifier of the created resource type ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": 0,
    "error_description": "Required fields: code"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `0` | `Required fields: code` | Required parameter not provided within `fields` ||
|| `100` | `Could not find value for parameter ` | Required parameter not provided ||
|| `422` | `Invalid value of the field` | Invalid field value ||
|| `1010` | `Resource type with code already exists'` | Resource type with this `code` already exists ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](../index.md)
- [{#T}](./booking-v1-resourcetype-get.md)
- [{#T}](./booking-v1-resourcetype-update.md)
- [{#T}](./booking-v1-resourcetype-delete.md)
- [{#T}](./booking-v1-resourcetype-list.md)