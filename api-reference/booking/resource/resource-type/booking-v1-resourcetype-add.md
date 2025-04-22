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
[`string`](../../../data-types.md) | The type of the booking message template. Possible values:
- `inanimate` — template for booking equipment and rooms
- `animate` — template for booking specialists

Default is `inanimate` ||
|| **isConfirmationNotificationOn**
[`string`](../../../data-types.md) | Automatic confirmation of the booking. Possible values:
- `Y` — enabled
- `N` — disabled

Default is `Y` ||
|| **templateTypeConfirmation**
[`string`](../../../data-types.md) | The type of the confirmation message template. Possible values:
- `inanimate` — template for booking equipment and rooms
- `animate` — template for booking specialists

Default is `inanimate` ||
|| **isReminderNotificationOn**
[`string`](../../../data-types.md) | Reminder about the booking. Possible values:
- `Y` — enabled
- `N` — disabled

Default is `Y` ||
|| **templateTypeReminder**
[`string`](../../../data-types.md) | The type of the reminder message template. Possible values: `base` ||
|| **isFeedbackNotificationOn**
[`string`](../../../data-types.md) | Request for feedback. Possible values:
- `Y` — enabled
- `N` — disabled

Default is `Y` ||
|| **templateTypeFeedback**
[`string`](../../../data-types.md) | The type of the feedback request message template. Possible values:
- `inanimate` — template for booking equipment and rooms
- `animate` — template for booking specialists

Default is `inanimate` ||
|| **isDelayedNotificationOn**
[`string`](../../../data-types.md) | Reminder when the client is late. Possible values:
- `Y` — enabled
- `N` — disabled

Default is `Y`||
|| **templateTypeDelayed**
[`string`](../../../data-types.md) | The type of the delay message template. Possible values:
- `inanimate` — template for booking equipment and rooms
- `animate` — template for booking specialists

Default is `inanimate` ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- JS

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
    -d '{"FIELDS":{"name":"Name","code":"code","isInfoNotificationOn":"Y","templateTypeInfo":"inanimate","isConfirmationNotificationOn":"Y","templateTypeConfirmation":"animate","isReminderNotificationOn":"N","templateTypeReminder":"base","isFeedbackNotificationOn":"Y","templateTypeFeedback":"inanimate","isDelayedNotificationOn":"Y","templateTypeDelayed":"inanimate"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/booking.v1.resourceType.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"FIELDS":{"name":"Name","code":"code","isInfoNotificationOn":"Y","templateTypeInfo":"inanimate","isConfirmationNotificationOn":"Y","templateTypeConfirmation":"animate","isReminderNotificationOn":"N","templateTypeReminder":"base","isFeedbackNotificationOn":"Y","templateTypeFeedback":"inanimate","isDelayedNotificationOn":"Y","templateTypeDelayed":"inanimate"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/booking.v1.resourceType.add
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'booking.v1.resourceType.add',
        [
            'FIELDS' => [
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
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
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
|| `0` | `Required fields: code` | The required parameter inside `fields` was not provided ||
|| `100` | `Could not find value for parameter ` | The required parameter was not provided ||
|| `422` | `Invalid value of the field` | Invalid field value ||
|| `1010` | `Resource type with code already exists'` | A resource type with this `code` already exists ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](../index.md)
- [{#T}](./booking-v1-resourcetype-get.md)
- [{#T}](./booking-v1-resourcetype-update.md)
- [{#T}](./booking-v1-resourcetype-delete.md)
- [{#T}](./booking-v1-resourcetype-list.md)