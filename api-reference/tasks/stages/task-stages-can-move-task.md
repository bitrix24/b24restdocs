# Check the ability to move a task task.stages.canmovetask

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method:
> - any user for stages of "My Planner"
> - any user with access to the group for kanban stages

The method checks whether the current user can move tasks in the specified object.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **entityId***
[`integer`](../../data-types.md) | `ID` of the object ||
|| **entityType***
[`string`](../../data-types.md) | Type of the object: 
- `U` — user
- `G` — group

In the case of `U` ("My Planner") — the value `true` will be returned only if the `entityId` contains the identifier of the current user ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
    "entityId": 1,
    "entityType": "U"
    }' \
    https://your-domain.bitrix24.com/rest/_USER_ID_/_CODE_/task.stages.canmovetask
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Authorization: YOUR_ACCESS_TOKEN" \
    -d '{
    "entityId": 1,
    "entityType": "U"
    }' \
    https://your-domain.bitrix24.com/rest/task.stages.canmovetask
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'task.stages.canmovetask',
    		{
    			entityId: entityId,
    			entityType: entityType
    		}
    	);
    	
    	const result = response.getData().result;
    	console.log(result);
    }
    catch( error )
    {
    	console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $entityId = 1;
        $entityType = 'U';
    
        $response = $b24Service
            ->core
            ->call(
                'task.stages.canmovetask',
                [
                    'entityId' => $entityId,
                    'entityType' => $entityType
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error calling task.stages.canmovetask: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    const entityId = 1;
    const entityType = 'U';
    BX24.callMethod(
        'task.stages.canmovetask',
        {
            entityId: entityId,
            entityType: entityType
        },
        function(res)
        {
            console.log(res);
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php'); // include CRest PHP SDK

    $entityId = 1;
    $entityType = 'U';

    // execute request to REST API
    $result = CRest::call(
        'task.stages.canmovetask',
        [
            'entityId' => $entityId,
            'entityType' => $entityType
        ]
    );

    // Process the response from Bitrix24
    if ($result['error']) {
        echo 'Error: '.$result['error_description'];
    } else {
        print_r($result['result']);
    }
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": true
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result** 
[`boolean`](../../data-types.md) | Returns `true` if the current user can move the task.

Otherwise — `false`
||
|#

## Error Handling

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./task-stages-add.md)
- [{#T}](./task-stages-update.md)
- [{#T}](./task-stages-get.md)
- [{#T}](./task-stages-move-task.md)
- [{#T}](./task-stages-delete.md)