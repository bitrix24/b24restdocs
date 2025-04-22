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
[`string`](../../data-types.md) | The type of message template for the booking. Possible values:
- `inanimate` — template for booking equipment and rooms
- `animate` — template for booking specialists 
  
Defaults to `inanimate` ||
|| **isConfirmationNotificationOn**
[`string`](../../data-types.md) | Automatic booking confirmation. Possible values: 
- `Y` — enabled 
- `N` — disabled
  
Defaults to `Y` ||
|| **templateTypeConfirmation**
[`string`](../../data-types.md) | The type of message template for booking confirmation. Possible values:
- `inanimate` — template for booking equipment and rooms
- `animate` — template for booking specialists 
  
Defaults to `inanimate`  ||
|| **isReminderNotificationOn**
[`string`](../../data-types.md) | Reminder about the booking. Possible values: 
- `Y` — enabled 
- `N` — disabled
  
Defaults to `Y` ||
|| **templateTypeReminder**
[`string`](../../data-types.md) | The type of message template for reminders. Possible values: `base` ||
|| **isFeedbackNotificationOn**
[`string`](../../data-types.md) | Feedback request. Possible values: 
- `Y` — enabled 
- `N` — disabled
  
Defaults to `Y` ||
|| **templateTypeFeedback**
[`string`](../../data-types.md) | The type of message template for feedback requests. Possible values:
- `inanimate` — template for booking equipment and rooms
- `animate` — template for booking specialists 
  
Defaults to `inanimate`  ||
|| **isDelayedNotificationOn**
[`string`](../../data-types.md) | Reminder when the client is late. Possible values: 
- `Y` — enabled 
- `N` — disabled
  
Defaults to `Y`||
|| **templateTypeDelayed**
[`string`](../../data-types.md) | The type of message template for delays. Possible values:
- `inanimate` — template for booking equipment and rooms
- `animate` — template for booking specialists 
  
Defaults to `inanimate`  ||
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
    -d '{"FIELDS":{"name":"Name","description":"Description","typeId":1,"isMain":"N","isInfoNotificationOn":"Y","templateTypeInfo":"inanimate","isConfirmationNotificationOn":"Y","templateTypeConfirmation":"animate","isReminderNotificationOn":"N","templateTypeReminder":"base","isFeedbackNotificationOn":"Y","templateTypeFeedback":"inanimate","isDelayedNotificationOn":"Y","templateTypeDelayed":"inanimate"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/booking.v1.resource.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"FIELDS":{"name":"Name","description":"Description","typeId":1,"isMain":"N","isInfoNotificationOn":"Y","templateTypeInfo":"inanimate","isConfirmationNotificationOn":"Y","templateTypeConfirmation":"animate","isReminderNotificationOn":"N","templateTypeReminder":"base","isFeedbackNotificationOn":"Y","templateTypeFeedback":"inanimate","isDelayedNotificationOn":"Y","templateTypeDelayed":"inanimate"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/booking.v1.resource.add
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'booking.v1.resource.add',
        [
            'FIELDS' => [
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
|| `0` | `Required fields: name` | Required parameter not provided within `fields` ||
|| `100` | `Could not find value for parameter {fields}` | Required parameter `fields` not provided ||
|| `1006` | `ResourceType not found` | `typeId` not specified ||
|| `1013` | `Resource type with id does not exist` | Non-existent `typeId` specified ||
|| `422` | `Invalid value of the field` | Invalid field value ||
|#

{% include [system errors](./../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./resource-type/index.md)
- [{#T}](./booking-v1-resource-get.md)
- [{#T}](./booking-v1-resource-update.md)
- [{#T}](./booking-v1-resource-delete.md)
- [{#T}](./booking-v1-resource-list.md)