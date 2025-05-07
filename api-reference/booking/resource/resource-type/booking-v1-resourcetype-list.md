# Get a list of resource types booking.v1.resourceType.list

> Scope: [`booking`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `booking.v1.resourceType.list` returns a list of resource types based on a filter. It is an implementation of the list method for resource types.

## Method Parameters

#|
|| **FILTER**
[`object`](../../../data-types.md) | Object for filtering the list of resource types in the format `{"field_1": "value_1", ... "field_N": "value_N"}`, where
- `field_N` — [field](#filter) of the resource type for filtering
- `value_N` — field value ||
|| **ORDER**
[`object`](../../../data-types.md) | Object for sorting the list of resource types in the format `{"field_1": "value_1", ... "field_N": "value_N"}`, where
- `field_N` — [field](#order) of the resource type for sorting
- `value_N` — sorting direction

The sorting direction can take the following values:
- `asc` — ascending
- `desc` — descending
  
The default value is `{ID: 'ASC'}` ||
|#

### Filter Parameters {#filter}

#|
|| **Name**
`type` | **Description** ||
|| **searchQuery**
[`string`](../../../data-types.md) | Search query. Searches by substring in the resource type name ||
|| **moduleId**
[`string`](../../../data-types.md) | Resource type module ||
|| **name**
[`string`](../../../data-types.md) | Resource type name ||
|| **code**
[`string`](../../../data-types.md) | Resource type code ||
|#

Use either `searchQuery` for substring search or `name` for exact match search.

### Order Parameters {#order}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../../data-types.md) | Sort by identifier ||
|| **name**
[`string`](../../../data-types.md) | Sort by name ||
|| **code**
[`string`](../../../data-types.md) | Sort by code ||
|#

## Code Examples

{% include [Examples Note](../../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        "booking.v1.resourceType.list",
        {
            filter: {
                    "searchQuery": "res",
                    "moduleId": "booking"
        },
        order: {
                id: "ASC",
                name: "DESC",
                code: "DESC"
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
    -d '{"filter":{"searchQuery":"res","moduleId":"booking"},"order":{"id":"ASC","name":"DESC","code":"DESC"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/booking.v1.resourceType.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":{"searchQuery":"res","moduleId":"booking"},"order":{"id":"ASC","name":"DESC","code":"DESC"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/booking.v1.resourceType.list
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'booking.v1.resourceType.list',
        [
            'filter' => [
                'searchQuery' => 'res',
                'moduleId' => 'booking'
            ],
            'order' => [
                'id' => 'ASC',
                'name' => 'DESC',
                'code' => 'DESC'
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
        "resource": [
            {
                "code": "equipment",
                "confirmationCounterDelay": 10800,
                "confirmationNotificationDelay": 86400,
                "confirmationNotificationRepetitions": null,
                "confirmationNotificationRepetitionsInterval": 10800,
                "delayedCounterDelay": 300,
                "delayedNotificationDelay": 300,
                "id": 3,
                "infoNotificationDelay": null,
                "isConfirmationNotificationOn": "Y",
                "isDelayedNotificationOn": "Y",
                "isFeedbackNotificationOn": "N",
                "isReminderNotificationOn": "Y",
                "name": "resource",
                "reminderNotificationDelay": -1,
                "templateTypeConfirmation": "inanimate",
                "templateTypeDelayed": "inanimate",
                "templateTypeFeedback": "inanimate",
                "templateTypeReminder": "base"
            },
            {
                "code": "expert",
                "confirmationCounterDelay": 10800,
                "confirmationNotificationDelay": 86400,
                "confirmationNotificationRepetitions": null,
                "confirmationNotificationRepetitionsInterval": 10800,
                "delayedCounterDelay": 300,
                "delayedNotificationDelay": 300,
                "id": 5,
                "infoNotificationDelay": null,
                "isConfirmationNotificationOn": "Y",
                "isDelayedNotificationOn": "Y",
                "isFeedbackNotificationOn": "N",
                "isReminderNotificationOn": "Y",
                "name": "resource 2",
                "reminderNotificationDelay": -1,
                "templateTypeConfirmation": "animate",
                "templateTypeDelayed": "animate",
                "templateTypeFeedback": "animate",
                "templateTypeReminder": "base"
            }
        ]
    },
    "time": {
        "start": 1746540063.20403,
        "finish": 1746540063.261006,
        "duration": 0.0569760799407959,
        "processing": 0.020888090133666992,
        "date_start": "2025-05-06T17:01:03+02:00",
        "date_finish": "2025-05-06T17:01:03+02:00",
        "operating_reset_at": 1746540663,
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

Contains an array of objects with information about resource types. The structure is described [below](#resource) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

#### Type {#resource}

#|
|| **code**
[`string`](../../../data-types.md) | Resource type code ||
|| **confirmationCounterDelay**
[`integer`](../../../data-types.md) | Time until the unconfirmed booking counter is activated in seconds ||
|| **confirmationDelay**
[`integer`](../../../data-types.md) | Time until the first confirmation message is sent to the client in seconds ||
|| **confirmationRepetitions**
[`integer`](../../../data-types.md) | Number of messages sent to the client for confirmation, excluding the first one ||
|| **confirmationRepetitionsInterval**
[`integer`](../../../data-types.md) | Interval between confirmation messages, in seconds ||
|| **delayedCounterDelay**
[`integer`](../../../data-types.md) | Time in seconds after which to activate the counter in the calendar ||
|| **delayedDelay**
[`integer`](../../../data-types.md) | Time in seconds after which to send a delay message to the client ||
|| **id**
[`integer`](../../../data-types.md) | Resource type identifier ||
|| **infoDelay**
[`integer`](../../../data-types.md) | Delay in seconds after which the client receives a booking message ||
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
[`string`](../../../data-types.md) | Resource name ||
|| **reminderDelay**
[`integer`](../../../data-types.md) | Time until the client receives a reminder about the booking in seconds.
Value `-1` means in the morning on the day of the booking ||
|| **templateTypeConfirmation**
[`string`](../../../data-types.md) | Type of confirmation message template. Possible values:
- `inanimate` — template for booking equipment and rooms
- `animate` — template for booking specialists ||
|| **templateTypeDelayed**
[`string`](../../../data-types.md) | Type of delay message template. Possible values:
- `inanimate` — template for booking equipment and rooms
- `animate` — template for booking specialists ||
|| **templateTypeFeedback**
[`string`](../../../data-types.md) | Type of feedback request message template. Possible values:
- `inanimate` — template for booking equipment and rooms
- `animate` — template for booking specialists ||
|| **templateTypeReminder**
[`string`](../../../data-types.md) | Type of reminder message template. Possible values: `base` ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "",
    "error_description": "Invalid value {ASC} to match with parameter {order}. Should be value of type array."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `100` | `Invalid value to match with parameter {order}. Should be value of type array` | The `order` parameter is not an object ||
|| `100` | `Invalid value to match with parameter {filter}. Should be value of type array` | The `filter` parameter is not an object ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](../index.md)
- [{#T}](./booking-v1-resourcetype-add.md)
- [{#T}](./booking-v1-resourcetype-update.md)
- [{#T}](./booking-v1-resourcetype-delete.md)
- [{#T}](./booking-v1-resourcetype-get.md)