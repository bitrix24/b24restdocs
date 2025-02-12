# Add Universal CRM Activity crm.activity.todo.add

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: a user with edit access permission for the CRM entity to which the activity is being added.

The method `crm.activity.todo.add` adds a universal activity to the timeline.

## Method Parameters

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ownerTypeId***
[`integer`](../../../../data-types.md) | [Identifier of the CRM object type](../../../data-types.md#object_type) to which the activity is linked, for example `2` for a deal ||
|| **ownerId***
[`integer`](../../../../data-types.md) | Identifier of the CRM entity to which the activity is linked, for example, `1` ||
|| **deadline***
[`datetime`](../../../../data-types.md) | Deadline for the activity, for example `2025-02-03T15:00:00` ||
|| **title**
[`string`](../../../../data-types.md) | Title of the activity, default is an empty string ||
|| **description**
[`string`](../../../../data-types.md) | Description of the activity, default is an empty string ||
|| **responsibleId**
[`integer`](../../../../data-types.md) | Identifier of the user responsible for the activity, for example `1` ||
|| **parentActivityId**
[`integer`](../../../../data-types.md) | Identifier of the activity in the timeline that can be linked to the created activity, for example `888` ||
|| **pingOffsets**
[`array`](../../../../data-types.md) | An array containing integer values in minutes to set reminder times for the activity. For example, `[0, 15]` means that 2 reminders will be created, one 15 minutes before the deadline and one at the moment the deadline occurs. Default is an empty array, with no reminders ||
|| **colorId**
[`integer`](../../../../data-types.md) | Identifier of the activity color in the timeline, for example `1`. There are 8 colors available, values from 1 to 7 and a default color if none is specified

||
|#

## Code Examples

{% include [Note on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ownerTypeId":2,"ownerId":1,"deadline":"'"$(date -Iseconds)"'","title":"Activity Title","description":"Activity Description","responsibleId":5,"pingOffsets":[0,15],"colorId":2}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.activity.todo.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ownerTypeId":2,"ownerId":1,"deadline":"'"$(date -Iseconds)"'","title":"Activity Title","description":"Activity Description","responsibleId":5,"pingOffsets":[0,15],"colorId":2,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.activity.todo.add
    ```

- JS

    ```js
    BX24.callMethod(
        "crm.activity.todo.add",
        {
            ownerTypeId: 2,
            ownerId: 1,
            deadline: (new Date()),
            title: 'Activity Title',
            description: 'Activity Description',
            responsibleId: 5,
            pingOffsets: [0, 15],
            colorId: 2
        }, 
        result => {
            if (result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.activity.todo.add',
        [
            'ownerTypeId' => 2,
            'ownerId' => 1,
            'deadline' => date('c'), // Current date and time in ISO 8601 format
            'title' => 'Activity Title',
            'description' => 'Activity Description',
            'responsibleId' => 5,
            'pingOffsets' => [0, 15],
            'colorId' => 2
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
        "id": 999
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
[`object`](../../../../data-types.md) | On success, returns an object containing the identifier of the added activity `id`, on error = `null` ||
|| **time**
[`time`](../../../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "NOT_FOUND",
    "error_description": "Not found."
}
```

{% include notitle [error handling](../../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `100` | Required fields not provided ||
|| `NOT_FOUND` | CRM entity not found ||
|| `ACCESS_DENIED` | Insufficient permissions to perform the operation ||
|| `OWNER_NOT_FOUND` | Owner of the entity not found ||
|| `WRONG_DATETIME_FORMAT` | Incorrect date format ||
|#

{% include [system errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-activity-todo-update.md)
- [{#T}](./crm-activity-todo-update-deadline.md)
- [{#T}](./crm-activity-todo-update-description.md)
- [{#T}](./crm-activity-todo-update-color.md)
- [{#T}](./crm-activity-todo-update-responsible-user.md)