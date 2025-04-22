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
Can be obtained through the methods [booking.v1.resource.add](./booking-v1-resource-add.md) and [booking.v1.resource.list](./booking-v1-resource-list.md) ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- JS

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

- PHP

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
        "name": "Name",
        "description": "Description",
        "typeId": 1,
        "isMain": "N",
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
[`object`](../../data-types.md) | Root element of the response. Contains information about the resource fields. The structure is described [below](#resource) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

#### Resource {#resource}

#|
|| **Name**
`type` | **Description** ||
|| **name**
[`string`](../../data-types.md) | Resource name ||
|| **description**
[`string`](../../data-types.md) | Resource description ||
|| **typeId**
[`integer`](../../data-types.md) | Resource type identifier. 

Information about the type can be obtained using the method [booking.v1.resourceType.get](./resource-type/booking-v1-resourcetype-get.md) ||
|| **isMain**
[`string`](../../data-types.md) | How to display the resource. Possible values:
- `Y` — in schedule columns
- `N` — when resources overlap
||
|| **isInfoNotificationOn**
[`string`](../../data-types.md) | Message to the client about the booking. Possible values:
- `Y` — enabled
- `N` — disabled ||
|| **templateTypeInfo**
[`string`](../../data-types.md) | Type of the booking message template. Possible values:
- `inanimate` — template for booking equipment and rooms
- `animate` — template for booking specialists ||
|| **isConfirmationNotificationOn**
[`string`](../../data-types.md) | Automatic confirmation of the booking. Possible values:
- `Y` — enabled
- `N` — disabled ||
|| **templateTypeConfirmation**
[`string`](../../data-types.md) | Type of the confirmation message template. Possible values:
- `inanimate` — template for booking equipment and rooms
- `animate` — template for booking specialists ||
|| **isReminderNotificationOn**
[`string`](../../data-types.md) | Reminder about the booking. Possible values:
- `Y` — enabled
- `N` — disabled ||
|| **templateTypeReminder**
[`string`](../../data-types.md) | Type of the reminder message template. Possible values: `base` ||
|| **isFeedbackNotificationOn**
[`string`](../../data-types.md) | Request for feedback. Possible values:
- `Y` — enabled
- `N` — disabled ||
|| **templateTypeFeedback**
[`string`](../../data-types.md) | Type of the feedback request message template. Possible values:
- `inanimate` — template for booking equipment and rooms
- `animate` — template for booking specialists ||
|| **isDelayedNotificationOn**
[`string`](../../data-types.md) | Reminder when the client is late. Possible values:
- `Y` — enabled
- `N` — disabled ||
|| **templateTypeDelayed**
[`string`](../../data-types.md) | Type of the delay message template. Possible values:
- `inanimate` — template for booking equipment and rooms
- `animate` — template for booking specialists ||
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