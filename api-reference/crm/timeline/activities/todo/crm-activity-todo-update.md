# Update universal activity crm.activity.todo.update

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: a user with edit access permission for the CRM entity associated with the activity being updated.

The method `crm.activity.todo.update` updates a universal activity.

## Method Parameters

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../../data-types.md) | Identifier of the activity being updated, for example `999` ||
|| **ownerTypeId***
[`integer`](../../../../data-types.md) | [Identifier of the CRM object type](../../../data-types.md#object_type) to which the activity is linked, for example `2` for a deal ||
|| **ownerId***
[`integer`](../../../../data-types.md) | Identifier of the CRM entity to which the activity is linked, for example `1` ||
|| **deadline***
[`datetime`](../../../../data-types.md) | Deadline of the activity, for example `2025-02-03T15:00:00` ||
|| **title**
[`string`](../../../../data-types.md) | Title of the activity ||
|| **description**
[`string`](../../../../data-types.md) | Description of the activity ||
|| **responsibleId**
[`integer`](../../../../data-types.md) | Identifier of the user responsible for the activity, for example `1` ||
|| **parentActivityId**
[`integer`](../../../../data-types.md) | Identifier of the activity in the timeline that can be linked to the updated activity, for example `888` ||
|| **pingOffsets**
[`array`](../../../../data-types.md) | An array containing integer values in minutes that allow setting reminder times for the activity. For example, `[0, 15]` means that 2 reminders will be created, one for 15 minutes before the deadline and one at the moment the deadline occurs. By default, an empty array with no reminders ||
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
    -d '{"id":999,"ownerTypeId":2,"ownerId":1,"deadline":"**put_current_date_here**","title":"New activity title","description":"New activity description","responsibleId":1,"pingOffsets":[15,30],"colorId":7}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.activity.todo.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":999,"ownerTypeId":2,"ownerId":1,"deadline":"**put_current_date_here**","title":"New activity title","description":"New activity description","responsibleId":1,"pingOffsets":[15,30],"colorId":7,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.activity.todo.update
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.activity.todo.update',
    		{
    			id: 999,
    			ownerTypeId: 2,
    			ownerId: 1,
    			deadline: (new Date()),
    			title: 'New activity title',
    			description: 'New activity description',
    			responsibleId: 1,
    			pingOffsets: [15, 30],
    			colorId: 7
    		}
    	);
    	
    	const result = response.getData().result;
    	if (result.error())
    		console.error(result.error());
    	else
    		console.dir(result);
    }
    catch( error )
    {
    	console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.activity.todo.update',
                [
                    'id'            => 999,
                    'ownerTypeId'   => 2,
                    'ownerId'       => 1,
                    'deadline'      => (new DateTime()),
                    'title'         => 'New activity title',
                    'description'   => 'New activity description',
                    'responsibleId' => 1,
                    'pingOffsets'   => [15, 30],
                    'colorId'       => 7
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Success: ' . print_r($result->data(), true);
            // Your logic for processing data
            processData($result->data());
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating todo activity: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.activity.todo.update",
        {
            id: 999,
            ownerTypeId: 2,
            ownerId: 1,
            deadline: (new Date()),
            title: 'New activity title',
            description: 'New activity description',
            responsibleId: 1,
            pingOffsets: [15, 30],
            colorId: 7
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
        'crm.activity.todo.update',
        [
            'id' => 999,
            'ownerTypeId' => 2,
            'ownerId' => 1,
            'deadline' => date('c'), // Assuming you want the current date in ISO 8601 format
            'title' => 'New activity title',
            'description' => 'New activity description',
            'responsibleId' => 1,
            'pingOffsets' => [15, 30],
            'colorId' => 7
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
[`object`](../../../../data-types.md) | On success, returns an object containing the integer identifier of the updated activity `id`, on error = `null` ||
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
|| `CAN_NOT_UPDATE_COMPLETED_TODO` | Completed activity cannot be modified ||
|| `ERROR_PARENT_ACTIVITY_RESTRICT` | Cannot schedule an activity for a closed activity ||
|#

{% include [system errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-activity-todo-add.md)
- [{#T}](./crm-activity-todo-update-deadline.md)
- [{#T}](./crm-activity-todo-update-description.md)
- [{#T}](./crm-activity-todo-update-color.md)
- [{#T}](./crm-activity-todo-update-responsible-user.md)