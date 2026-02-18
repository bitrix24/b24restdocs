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
[`string`](../../data-types.md) | Search query. Searches by substring in the resource name ||
|| **isMain**
[`string`](../../data-types.md) | Filter by resource display setting. Possible values:
- `Y` — in schedule columns
- `N` — when resources overlap ||
|| **typeId**
[`integer`](../../data-types.md) | Identifier of the resource type.

The list of available types can be obtained using the method [booking.v1.resourceType.list](./resource-type/booking-v1-resourcetype-list.md) ||
|| **name**
[`string`](../../data-types.md) | Name of the resource ||
|| **description**
[`string`](../../data-types.md) | Description of the resource ||
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

{% include [Examples Note](../../../_includes/examples.md) %}

{% list tabs %}

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

- JS

    ```js
    // callListMethod: Retrieves all data at once. Use only for small selections (< 1000 items) due to high memory usage.
    
    try {
      const response = await $b24.callListMethod(
        'booking.v1.resource.list',
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
        (progress) => { console.log('Progress:', progress) }
      );
      const items = response.getData() || [];
      for (const entity of items) { console.log('Entity:', entity); }
    } catch (error) {
      console.error('Request failed', error);
    }
    
    // fetchListMethod: Retrieves data in parts using an iterator. Use it for large data volumes to optimize memory usage.
    
    try {
      const generator = $b24.fetchListMethod('booking.v1.resource.list', {
        filter: {
          "searchQuery": "car",
          "isMain": "Y",
          "typeId": 1
        },
        order: {
          id: "ASC",
          name: "DESC"
        }
      }, 'ID');
      for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity); }
      }
    } catch (error) {
      console.error('Request failed', error);
    }
    
    // callMethod: Manually controls pagination through the start parameter. Use it for precise control of request batches. For large datasets, it is less efficient than fetchListMethod.
    
    try {
      const response = await $b24.callMethod('booking.v1.resource.list', {
        filter: {
          "searchQuery": "car",
          "isMain": "Y",
          "typeId": 1
        },
        order: {
          id: "ASC",
          name: "DESC"
        }
      }, 0);
      const result = response.getData().result || [];
      for (const entity of result) { console.log('Entity:', entity); }
    } catch (error) {
      console.error('Request failed', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'booking.v1.resource.list',
                [
                    'filter' => [
                        'searchQuery' => 'car',
                        'isMain'      => 'Y',
                        'typeId'      => 1,
                    ],
                    'order' => [
                        'id'   => 'ASC',
                        'name' => 'DESC',
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
            echo 'Error: ' . $result->error();
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error calling booking.v1.resource.list: ' . $e->getMessage();
    }
    ```

- BX24.js

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

- PHP CRest

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
                "confirmationCounterDelay": 10800,
                "confirmationNotificationDelay": 86400,
                "confirmationNotificationRepetitions": null,
                "confirmationNotificationRepetitionsInterval": 10800,
                "delayedCounterDelay": 300,
                "delayedNotificationDelay": 300,
                "description": null,
                "id": 5,
                "infoNotificationDelay": null,
                "isConfirmationNotificationOn": "Y",
                "isDelayedNotificationOn": "Y",
                "isFeedbackNotificationOn": "N",
                "isInfoNotificationOn": "Y",
                "isMain": "Y",
                "isReminderNotificationOn": "Y",
                "name": "Sedan 1",
                "reminderNotificationDelay": -1,
                "templateTypeConfirmation": "inanimate",
                "templateTypeDelayed": "inanimate",
                "templateTypeFeedback": "inanimate",
                "templateTypeInfo": "inanimate",
                "templateTypeReminder": "base",
                "typeId": 1
            },
            {
                "confirmationCounterDelay": 10800,
                "confirmationNotificationDelay": 86400,
                "confirmationNotificationRepetitions": null,
                "confirmationNotificationRepetitionsInterval": 10800,
                "delayedCounterDelay": 300,
                "delayedNotificationDelay": 300,
                "description": null,
                "id": 7,
                "infoNotificationDelay": null,
                "isConfirmationNotificationOn": "Y",
                "isDelayedNotificationOn": "Y",
                "isFeedbackNotificationOn": "N",
                "isInfoNotificationOn": "Y",
                "isMain": "Y",
                "isReminderNotificationOn": "Y",
                "name": "Sedan 2",
                "reminderNotificationDelay": -1,
                "templateTypeConfirmation": "inanimate",
                "templateTypeDelayed": "inanimate",
                "templateTypeFeedback": "inanimate",
                "templateTypeInfo": "inanimate",
                "templateTypeReminder": "base",
                "typeId": 1
            }
        ]
    },
    "time": {
        "start": 1746540454.261779,
        "finish": 1746540454.303483,
        "duration": 0.04170393943786621,
        "processing": 0.009412050247192383,
        "date_start": "2025-05-06T17:07:34+02:00",
        "date_finish": "2025-05-06T17:07:34+02:00",
        "operating_reset_at": 1746541054,
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
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

#### Resource {#resource}

#|
|| **confirmationCounterDelay**
[`integer`](../../data-types.md) | Time until the record in seconds, after which the unconfirmed record counter lights up ||
|| **confirmationDelay**
[`integer`](../../data-types.md) | Time until the record in seconds, when the client receives the first message for confirmation ||
|| **confirmationRepetitions**
[`integer`](../../data-types.md) | Number of messages that the client receives for confirmation, excluding the first one ||
|| **confirmationRepetitionsInterval**
[`integer`](../../data-types.md) | Interval between confirmation messages, in seconds ||
|| **delayedCounterDelay**
[`integer`](../../data-types.md) | Time in seconds after which to turn on the counter in the calendar ||
|| **delayedDelay**
[`integer`](../../data-types.md) | Time in seconds after which to send a message to the client about the delay ||
|| **description**
[`string`](../../data-types.md) | Description of the resource ||
|| **id**
[`integer`](../../data-types.md) | Identifier of the resource ||
|| **infoDelay**
[`integer`](../../data-types.md) | Delay in seconds after which the client receives a message about the record ||
|| **isConfirmationNotificationOn**
[`string`](../../data-types.md) | Automatic confirmation of the record. Possible values:
- `Y` — enabled
- `N` — disabled ||
|| **isDelayedNotificationOn**
[`string`](../../data-types.md) | Reminder when the client is late. Possible values:
- `Y` — enabled
- `N` — disabled ||
|| **isFeedbackNotificationOn**
[`string`](../../data-types.md) | Request for feedback. Possible values:
- `Y` — enabled
- `N` — disabled ||
|| **isInfoNotificationOn**
[`string`](../../data-types.md) | Message to the client about the record. Possible values:
- `Y` — enabled
- `N` — disabled ||
|| **isMain**
[`string`](../../data-types.md) | How to display the resource. Possible values:
- `Y` — in schedule columns
- `N` — when resources overlap ||
|| **isReminderNotificationOn**
[`string`](../../data-types.md) | Reminder about the record. Possible values:
- `Y` — enabled
- `N` — disabled ||
|| **name**
[`string`](../../data-types.md) | Name of the resource ||
|| **reminderDelay**
[`integer`](../../data-types.md) | Time until the record in seconds, for which the client receives a reminder about the record.
Value `-1` — in the morning on the day of the record ||
|| **templateTypeConfirmation**
[`string`](../../data-types.md) | Type of the confirmation message template. Possible values:
- `inanimate` — template for booking equipment and premises
- `animate` — template for appointments with specialists ||
|| **templateTypeDelayed**
[`string`](../../data-types.md) | Type of the delay message template. Possible values:
- `inanimate` — template for booking equipment and premises
- `animate` — template for appointments with specialists ||
|| **templateTypeFeedback**
[`string`](../../data-types.md) | Type of the feedback request message template. Possible values:
- `inanimate` — template for booking equipment and premises
- `animate` — template for appointments with specialists ||
|| **templateTypeInfo**
[`string`](../../data-types.md) | Type of the record message template. Possible values:
- `inanimate` — template for booking equipment and premises
- `animate` — template for appointments with specialists ||
|| **templateTypeReminder**
[`string`](../../data-types.md) | Type of the reminder message template. Possible values: `base` ||
|| **typeId**
[`integer`](../../data-types.md) | Identifier of the resource type.

Information about the type can be obtained using the method [booking.v1.resourceType.get](./resource-type/booking-v1-resourcetype-get.md) ||
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

