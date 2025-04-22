# Update Resource Type booking.v1.resourceType.update

> Scope: [`booking`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `booking.v1.resourceType.update` updates the information of an existing resource type.

## Method Parameters

{% include [Footnote on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of the resource type. 
Can be obtained through the methods [booking.v1.resourceType.add](./booking-v1-resourcetype-add.md) and [booking.v1.resourceType.list](./booking-v1-resourcetype-list.md) ||
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
|#

## Code Examples

{% include [Footnote on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- JS

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
                templateTypeDelayed: "animate"
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
    -d '{"id":10,"fields":{"name":"New Name","code":"Updated Code","isInfoNotificationOn":"Y","templateTypeInfo":"inanimate","isConfirmationNotificationOn":"Y","templateTypeConfirmation":"animate","isReminderNotificationOn":"Y","templateTypeReminder":"base","isFeedbackNotificationOn":"N","templateTypeFeedback":"animate","isDelayedNotificationOn":"N","templateTypeDelayed":"animate"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webbhook_here**/booking.v1.resourceType.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":10,"fields":{"name":"New Name","code":"Updated Code","isInfoNotificationOn":"Y","templateTypeInfo":"inanimate","isConfirmationNotificationOn":"Y","templateTypeConfirmation":"animate","isReminderNotificationOn":"Y","templateTypeReminder":"base","isFeedbackNotificationOn":"N","templateTypeFeedback":"animate","isDelayedNotificationOn":"N","templateTypeDelayed":"animate"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/booking.v1.resourceType.update
    ```

- PHP

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
                'templateTypeDelayed' => 'animate'
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
[`boolean`](../../../data-types.md) | Root element of the response, contains `true` in case of success ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

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
|| `1007` | `Resource type not found` | The specified resource type `id` does not exist ||
|| `100` | `Could not find value for parameter` | A required parameter was not provided ||
|| `422` | `Invalid value of the field` | Invalid field value ||
|| `1011` | `Resource type with code already exists` | A resource type with this `code` already exists ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](../index.md)
- [{#T}](./booking-v1-resourcetype-get.md)
- [{#T}](./booking-v1-resourcetype-add.md)
- [{#T}](./booking-v1-resourcetype-delete.md)
- [{#T}](./booking-v1-resourcetype-list.md)