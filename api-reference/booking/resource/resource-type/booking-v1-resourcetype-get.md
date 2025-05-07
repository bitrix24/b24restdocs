# Get Resource Type booking.v1.resourceType.get

> Scope: [`booking`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `booking.v1.resourceType.get` returns information about the resource type by its identifier.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of the resource type.
Can be obtained from the methods [booking.v1.resourceType.add](./booking-v1-resourcetype-add.md) and [booking.v1.resourceType.list](./booking-v1-resourcetype-list.md) ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- JS

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

- PHP

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
[`object`](../../../data-types.md) | The root element of the response. Contains information about the fields of the resource type. The structure is described [below](#resource) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

#### Resource Type {#resource}

#|
|| **code**
[`string`](../../../data-types.md) | Code of the resource type ||
|| **confirmationCounterDelay**
[`integer`](../../../data-types.md) | Time until the record in seconds, after which the unconfirmed record counter is activated ||
|| **confirmationDelay**
[`integer`](../../../data-types.md) | Time until the record in seconds, when the client receives the first message for confirmation of the record ||
|| **confirmationRepetitions**
[`integer`](../../../data-types.md) | Number of messages that the client receives for confirmation of the record, excluding the first one ||
|| **confirmationRepetitionsInterval**
[`integer`](../../../data-types.md) | Interval between confirmation messages, in seconds ||
|| **delayedCounterDelay**
[`integer`](../../../data-types.md) | Time in seconds after which to turn on the counter in the calendar ||
|| **delayedDelay**
[`integer`](../../../data-types.md) | Time in seconds after which to send a message to the client about the delay ||
|| **id**
[`integer`](../../../data-types.md) | Identifier of the resource ||
|| **infoDelay**
[`integer`](../../../data-types.md) | Delay in seconds after which the client receives a message about the record ||
|| **isConfirmationNotificationOn**
[`string`](../../../data-types.md) | Automatic confirmation of the record. Possible values:
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
[`string`](../../../data-types.md) | Reminder about the record. Possible values:
- `Y` — enabled
- `N` — disabled ||
|| **name**
[`string`](../../../data-types.md) | Name of the resource ||
|| **reminderDelay**
[`integer`](../../../data-types.md) | Time until the record in seconds, when the client receives a reminder about the record.
Value `-1` — in the morning on the day of the record ||
|| **templateTypeConfirmation**
[`string`](../../../data-types.md) | Type of the confirmation message template. Possible values:
- `inanimate` — template for booking equipment and rooms
- `animate` — template for appointments with specialists ||
|| **templateTypeDelayed**
[`string`](../../../data-types.md) | Type of the delay message template. Possible values:
- `inanimate` — template for booking equipment and rooms
- `animate` — template for appointments with specialists ||
|| **templateTypeFeedback**
[`string`](../../../data-types.md) | Type of the feedback request message template. Possible values:
- `inanimate` — template for booking equipment and rooms
- `animate` — template for appointments with specialists ||
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