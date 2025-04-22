# Get the list of resources booking.v1.resource.list

> Scope: [`booking`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `booking.v1.resource.list` returns a list of resources based on a filter. It is an implementation of the listing method for resources.

## Method Parameters

#|
|| **FILTER**
[`object`](../../data-types.md) | Object for filtering the list of resources in the format `{"field_1": "value_1", ... "field_N": "value_N"}`, where
- `field_N` — [field](#filter) of the resource for filtering
- `value_N` — value of the field ||
|| **ORDER**
[`object`](../../data-types.md) | Object for sorting the list of resources in the format `{"field_1": "value_1", ... "field_N": "value_N"}`, where
- `field_N` — [field](#order) of the resource for sorting
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
[`string`](../../data-types.md) | Search query. Searches for a substring in the resource name ||
|| **isMain**
[`string`](../../data-types.md) | Filter by resource display settings. Possible values:
- `Y` — in the schedule columns
- `N` — when resources overlap ||
|| **typeId**
[`integer`](../../data-types.md) | Resource type identifier.

The list of available types can be obtained using the method [booking.v1.resourceType.list](./resource-type/booking-v1-resourcetype-list.md) ||
|| **name**
[`string`](../../data-types.md) | Resource name ||
|| **description**
[`string`](../../data-types.md) | Resource description ||
|#

Use either `searchQuery` for substring search or `name` for exact match search.

### Order Parameters {#order}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../data-types.md) | Sort by identifier ||
|| **name**
[`string`](../../data-types.md) | Sort by name ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        "booking.v1.resource.list",
        {
            filter: {
                "searchQuery": "car",
                "isMain": "Y",
                "typeId": 1
            },
            order: {
                id: "ASC",
                name: "DESC"
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
    -d '{"filter":{"searchQuery":"car","isMain":"Y","typeId":1},"order":{"id":"ASC","name":"DESC"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/booking.v1.resource.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":{"searchQuery":"car","isMain":"Y","typeId":1},"order":{"id":"ASC","name":"DESC"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/booking.v1.resource.list
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'booking.v1.resource.list',
        [
            'filter' => [
                'searchQuery' => 'car',
                'isMain' => 'Y',
                'typeId' => 1
            ],
            'order' => [
                'id' => 'ASC',
                'name' => 'DESC'
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
             "description": "Sedan 1",
             "id": 1,
             "isConfirmationNotificationOn": "N",
             "isDelayedNotificationOn": "Y",
             "isFeedbackNotificationOn": "Y",
             "isInfoNotificationOn": "N",
             "isMain": "N",
             "isReminderNotificationOn": "Y",
             "name": "resource",
             "templateTypeConfirmation": "inanimate",
             "templateTypeDelayed": "inanimate",
             "templateTypeFeedback": "inanimate",
             "templateTypeInfo": "inanimate",
             "templateTypeReminder": "inanimate",
             "typeId": 2
         },
         {
             "description": "Sedan 2",
             "id": 2,
             "isConfirmationNotificationOn": "N",
             "isDelayedNotificationOn": "Y",
             "isFeedbackNotificationOn": "Y",
             "isInfoNotificationOn": "N",
             "isMain": "Y",
             "isReminderNotificationOn": "N",
             "name": "resource",
             "templateTypeConfirmation": "inanimate",
             "templateTypeDelayed": "inanimate",
             "templateTypeFeedback": "inanimate",
             "templateTypeInfo": "inanimate",
             "templateTypeReminder": "inanimate",
             "typeId": 2
         }
     ]
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
[`object`](../../data-types.md) | Root element of the response. 

Contains an array of objects with information about resources. The structure is described [below](#resource) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Resource {#resource}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Resource identifier ||
|| **name**
[`string`](../../data-types.md) | Resource name ||
|| **description**
[`string`](../../data-types.md) | Resource description ||
|| **typeId**
[`integer`](../../data-types.md) | Resource type identifier. 

Information about the type can be obtained using the method [booking.v1.resourceType.get](./resource-type/booking-v1-resourcetype-get.md) ||
|| **isMain**
[`string`](../../data-types.md) | How to display the resource. Possible values:
- `Y` — in the schedule columns
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
[`string`](../../data-types.md) | Automatic booking confirmation. Possible values:
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
    "error": "",
    "error_description": "Invalid value {ASC} to match with parameter {order}. Should be value of type array."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `100` | `Invalid value to match with parameter {order}. Should be value of type array` | The parameter `order` is not an object ||
|| `100` | `Invalid value to match with parameter {filter}. Should be value of type array` | The parameter `filter` is not an object ||
|#

{% include [system errors](./../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./resource-type/index.md)
- [{#T}](./booking-v1-resource-add.md)
- [{#T}](./booking-v1-resource-update.md)
- [{#T}](./booking-v1-resource-delete.md)