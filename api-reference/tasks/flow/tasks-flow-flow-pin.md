# Pin or Unpin Flow tasks.flow.Flow.pin

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: a user who can see the flow in the list of flows

This method pins or unpins a flow in the list of flows by its identifier.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **flowId*** 
[`integer`](../../data-types.md) | The identifier of the flow to be pinned or unpinned.

You can obtain the identifier using the method to create a new flow [tasks.flow.Flow.create](./tasks-flow-flow-create.md) or the method to get a task [tasks.task.get](../tasks-task-get.md) for a task from the flow ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
        "flowId": 517
    }' \
    https://your-domain.bitrix24.com/rest/_USER_ID_/_CODE_/tasks.flow.Flow.pin
    ```

- cURL (oAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
    -d '{
        "flowId": 517
    }' \
    https://your-domain.bitrix24.com/rest/tasks.flow.Flow.pin
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'tasks.flow.Flow.pin',
    		{
    			flowId: 517
    		}
    	);
    	
    	const result = response.getData().result;
    	console.info(result);
    }
    catch( error )
    {
    	console.error(error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'tasks.flow.Flow.pin',
                [
                    'flowId' => 517
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
            echo 'Error: ' . $result->error();
        } else {
            echo 'Info: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error calling tasks.flow.Flow.pin: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'tasks.flow.Flow.pin',
        {
            flowId: 517
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.info(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php'); // connecting CRest PHP SDK
    
    $flowId = 517;
    
    // executing request to REST API
    $result = CRest::call(
        'tasks.flow.Flow.pin',
        [
            'flowId' => $flowId
        ]
    );
    
    // Processing the response from Bitrix24
    if ($result['error']) {
        echo 'Error: '.$result['error_description'];
    } else {
        print_r($result['result']);
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

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
[`boolean`](../../data-types.md) | The result of the method execution. Possible values:
- `true` — flow is pinned
- `false` — flow is unpinned 
||
|#

## Error Handling

HTTP status: **200**

```json
{
    "error": "0",
    "error_description": "Unknown error"
}
```

### Possible Error Codes

#|
|| **Code** | **Description** | **Additional Information** ||
|| `0` | Access denied or flow not found | The account plan does not allow working with flows or the user does not have permission to perform the operation ||
|| `0` | Unknown error | Unknown error ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./tasks-flow-flow-create.md)
- [{#T}](./tasks-flow-flow-get.md)
- [{#T}](./tasks-flow-flow-update.md)
- [{#T}](./tasks-flow-flow-delete.md)
- [{#T}](./tasks-flow-flow-is-exists.md)
- [{#T}](./tasks-flow-flow-activate.md)