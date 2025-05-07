# Add a new resource booking.v1.resource.add

> Scope: [`booking`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `booking.v1.resource.add` adds a new resource.

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | An object containing field values for creating a resource [(detailed description)](#fields) ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../data-types.md) | The name of the resource ||
|| **description**
[`string`](../../data-types.md) | The description of the resource.

Defaults to an empty string ||
|| **typeId***
[`integer`](../../data-types.md) | The identifier of the resource type.

A list of available types can be obtained using the method [booking.v1.resourceType.list](./resource-type/booking-v1-resourcetype-list.md) ||
|| **isMain**
[`string`](../../data-types.md) | How to display the resource. Possible values:
- `Y` — in the schedule columns
- `N` — when resources overlap

Defaults to `Y` ||
|| **isInfoNotificationOn**
[`string`](../../data-types.md) | Message to the client about the booking. Possible values:
- `Y` — enabled
- `N` — disabled

Defaults to `Y` ||
|| **templateTypeInfo**
[`string`](../../data-types.md) | The type of the booking message template. Possible values:
- `inanimate` — template for booking equipment and rooms
- `animate` — template for booking specialists

Defaults to `inanimate` ||
|| **isConfirmationNotificationOn**
[`string`](../../data-types.md) | Automatic confirmation of the booking. Possible values:
- `Y` — enabled
- `N` — disabled

Defaults to `Y` ||
|| **templateTypeConfirmation**
[`string`](../../data-types.md) | The type of the confirmation message template. Possible values:
- `inanimate` — template for booking equipment and rooms
- `animate` — template for booking specialists

Defaults to `inanimate` ||
|| **isReminderNotificationOn**
[`string`](../../data-types.md) | Reminder about the booking. Possible values:
- `Y` — enabled
- `N` — disabled

Defaults to `Y` ||
|| **templateTypeReminder**
[`string`](../../data-types.md) | The type of the reminder message template. Possible values: `base` ||
|| **isFeedbackNotificationOn**
[`string`](../../data-types.md) | Feedback request. Possible values:
- `Y` — enabled
- `N` — disabled

Defaults to `Y` ||
|| **templateTypeFeedback**
[`string`](../../data-types.md) | The type of the feedback request message template. Possible values:
- `inanimate` — template for booking equipment and rooms
- `animate` — template for booking specialists

Defaults to `inanimate` ||
|| **isDelayedNotificationOn**
[`string`](../../data-types.md) | Reminder when the client is late. Possible values:
- `Y` — enabled
- `N` — disabled

Defaults to `Y`||
|| **templateTypeDelayed**
[`string`](../../data-types.md) | The type of the delay message template. Possible values:
- `inanimate` — template for booking equipment and rooms
- `animate` — template for booking specialists

Defaults to `inanimate` ||
|| **infoDelay**
[`integer`](../../data-types.md) | The delay after which the client receives a booking message. Specified in seconds.

Defaults to 300 ||
|| **reminderDelay**
[`integer`](../../data-types.md) | The time before the booking when the client receives a reminder. Specified in seconds.

Defaults to -1, in the morning on the day of the booking ||
|| **delayedDelay**
[`integer`](../../data-types.md) | The time after which to send a delay message to the client. Specified in seconds.

Defaults to 300 ||
|| **delayedCounterDelay**
[`integer`](../../data-types.md) | The time after which to enable the counter in the calendar. Specified in seconds.

Defaults to 7200 ||
|| **confirmationDelay**
[`integer`](../../data-types.md) | The time before the booking when the client receives the first confirmation message. Specified in seconds.

Defaults to 86400 ||
|| **confirmationRepetitions**
[`integer`](../../data-types.md) | The number of messages sent to the client for booking confirmation, excluding the first.

Defaults to 0 ||
|| **confirmationRepetitionsInterval**
[`integer`](../../data-types.md) | The interval between confirmation messages. Specified in seconds.

Defaults to 0 ||
|| **confirmationCounterDelay**
[`integer`](../../data-types.md) | The time before the booking after which the counter for unconfirmed bookings lights up. Specified in seconds.

Defaults to 7200 ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        "booking.v1.resource.add",
        {
            fields: {
                name: "Name",
                description: "Description",
                typeId: 1,
                isMain: "N",
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
                infoDelay: 60,
                reminderDelay: -1,
                delayedDelay: 300,
                delayedCounterDelay: 7200,
                confirmationDelay: 86400,
                confirmationRepetitions: 1,
                confirmationRepetitionsInterval: 3600,
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


- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"name":"Name","description":"Description","typeId":1,"isMain":"N","isInfoNotificationOn":"Y","templateTypeInfo":"inanimate","isConfirmationNotificationOn":"Y","templateTypeConfirmation":"animate","isReminderNotificationOn":"N","templateTypeReminder":"base","isFeedbackNotificationOn":"Y","templateTypeFeedback":"inanimate","isDelayedNotificationOn":"Y","templateTypeDelayed":"inanimate","infoDelay":60,"reminderDelay":-1,"delayedDelay":300,"delayedCounterDelay":7200,"confirmationDelay":86400,"confirmationRepetitions":1,"confirmationRepetitionsInterval":3600,"confirmationCounterDelay":7200}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/booking.v1.resource.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"name":"Name","description":"Description","typeId":1,"isMain":"N","isInfoNotificationOn":"Y","templateTypeInfo":"inanimate","isConfirmationNotificationOn":"Y","templateTypeConfirmation":"animate","isReminderNotificationOn":"N","templateTypeReminder":"base","isFeedbackNotificationOn":"Y","templateTypeFeedback":"inanimate","isDelayedNotificationOn":"Y","templateTypeDelayed":"inanimate","infoDelay":60,"reminderDelay":-1,"delayedDelay":300,"delayedCounterDelay":7200,"confirmationDelay":86400,"confirmationRepetitions":1,"confirmationRepetitionsInterval":3600,"confirmationCounterDelay":7200},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/booking.v1.resource.add
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'booking.v1.resource.add',
        [
            'fields' => [
                'name' => 'Name',
                'description' => 'Description',
                'typeId' => 1,
                'isMain' => 'N',
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
                'infoDelay' => 60,
                'reminderDelay' => -1,
                'delayedDelay' => 300,
                'delayedCounterDelay' => 7200,
                'confirmationDelay' => 86400,
                'confirmationRepetitions' => 1,
                'confirmationRepetitionsInterval' => 3600,
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

HTTP status: **200**

```json
{
    "result": {
        "id": 1823
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
[`object`](../../data-types.md) | The root element of the response, contains the identifier of the created resource ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": 1013,
    "error_description": "Resource type with id 17 does not exist"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `0` | `Required fields: name` | The required parameter inside `fields` is missing ||
|| `100` | `Could not find value for parameter {fields}` | The required parameter `fields` is missing ||
|| `1006` | `ResourceType not found` | `typeId` is not specified ||
|| `1013` | `Resource type with id does not exist` | A non-existent `typeId` is specified ||
|| `422` | `Invalid value of the field` | Invalid field value ||
|#

{% include [system errors](./../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./resource-type/index.md)
- [{#T}](./booking-v1-resource-get.md)
- [{#T}](./booking-v1-resource-update.md)
- [{#T}](./booking-v1-resource-delete.md)
- [{#T}](./booking-v1-resource-list.md)