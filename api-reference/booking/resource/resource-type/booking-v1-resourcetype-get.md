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
Can be obtained in the methods [booking.v1.resourceType.add](./booking-v1-resourcetype-add.md) and [booking.v1.resourceType.list](./booking-v1-resourcetype-list.md) ||
|#

## Code Examples

{% include [Footnote about examples](../../../../_includes/examples.md) %}

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

HTTP Status: **200**

```json
{
  "result": {
    "resource": {
            "name": "Name",
            "code": "code",
            "isInfoNotificationOn": "Y",
            "templateTypeInfo": "inanimate",
            "isConfirmationNotificationOn": "Y",
            "templateTypeConfirmation": "animate",
            "isReminderNotificationOn": "N",
            "templateTypeReminder": "base",
            "isFeedbackNotificationOn": "Y",
            "templateTypeFeedback": "inanimate",
            "isDelayedNotificationOn": "Y",
            "templateTypeDelayed": "inanimate"
    }
  },
  "time": {
    "start": 1741002780.099604,
    "finish": 1741002780.381558,
    "duration": 0.2819540500640869,
    "processing": 0.14896297454833984,
    "date_start": "2025-03-03T11:53:00+00:00",
    "date_finish": "2025-03-03T11:53:00+00:00",
    "operating": 0.14890098571777344
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
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

#### Resource Type {#resource}

#|
|| **Name**
`type` | **Description** ||
|| **name**
[`string`](../../../data-types.md) | Name of the resource ||
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
[`string`](../../../data-types.md) | Automatic confirmation of the booking. Possible values:
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
|#

## Error Handling

HTTP Status: **400**

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