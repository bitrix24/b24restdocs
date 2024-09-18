# Delete Flow tasks.flow.flow.delete

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: creator or administrator of the flow

The method `tasks.flow.flow.delete` removes a flow by its identifier.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **flowData*** 
[`object`](../../data-types.md) | Object containing data for deleting the flow ||
|| **flowData.id*** 
[`integer`](../../data-types.md) | Identifier of the flow to be deleted. 

You can obtain the identifier using the method [tasks.task.get](../tasks-task-get.md) for a task that has already been added to the flow, or create a new flow using the method [tasks.flow.flow.create](./tasks-flow-flow-create.md) ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
        "flowData": {
            "id": 517
        }
    }' \
    https://your-domain.bitrix24.com/rest/_USER_ID_/_CODE_/tasks.flow.flow.delete
    ```

- cURL (oAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
    -d '{
        "flowData": {
            "id": 517
        }
    }' \
    https://your-domain.bitrix24.com/rest/tasks.flow.flow.delete
    ```

- JS

    ```js
    BX24.callMethod(
        'tasks.flow.flow.delete',
        {
            flowData: {
                id: 517
            }
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

- PHP

    ```php
    require_once('crest.php'); // connecting CRest PHP SDK

    $flowData = [
        "id" => 517
    ];

    // executing the request to the REST API
    $result = CRest::call(
        'tasks.flow.flow.delete',
        [
            'flowData' => $flowData
        ]
    );

    // Handling the response from Bitrix24
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
    "result": {
        "deleted": true
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result** 
[`object`](../../data-types.md) | Object containing the result of the operation ||
|| **deleted** 
[`boolean`](../../data-types.md) | Success of the flow deletion ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "0",
    "error_description": "Flow not found"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Additional Information** ||
|| `0` | Access denied or flow not found | The account's plan may not allow working with flows or the user lacks permission to delete the flow ||
|| `0` | `Flow not found` | The flow with the specified identifier was not found ||
|| `0` | `Unknown error` | An unknown error occurred ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./tasks-flow-flow-create.md)
- [{#T}](./tasks-flow-flow-get.md)
- [{#T}](./tasks-flow-flow-update.md)
- [{#T}](./tasks-flow-flow-is-exists.md)
- [{#T}](./tasks-flow-flow-activate.md)