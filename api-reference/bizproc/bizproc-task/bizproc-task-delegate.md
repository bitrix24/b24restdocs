# Delegate a workflow task bizproc.task.delegate

> Scope: [`bizproc`](../../scopes/permissions.md)
>
> Who can execute the method: any user responsible for the workflow task

This method delegates a workflow task. You can only delegate your own task.

{% note tip "User documentation" %}

- [Actions: Tasks](https://helpdesk.bitrix24.com/open/11466058/)

{% endnote %}

## Method parameters

{% include [Footnote about parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **TASK_IDS***
[`array`](../../data-types.md) | List of task identifiers.

You can obtain identifiers using the [bizproc.task.list](./bizproc-task-list.md) method. ||
|| **FROM_USER_ID***
[`integer`](../../data-types.md) | Identifier of the user from whom the tasks will be delegated.

You can obtain the user identifier using the [user.get](../../user/user-get.md) method. ||
|| **TO_USER_ID***
[`integer`](../../data-types.md) | Identifier of the user to whom the tasks will be delegated.

You can obtain the user identifier using the [user.get](../../user/user-get.md) method.

Who you can delegate to depends on the task settings: only subordinates, all employees, or no one. ||
|#

## Code examples

{% include [Footnote about examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASK_IDS":[1128,1129,1130],"FROM_USER_ID":15,"TO_USER_ID":37}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/bizproc.task.delegate
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASK_IDS":[1128,1129,1130],"FROM_USER_ID":15,"TO_USER_ID":37,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/bizproc.task.delegate
    ```

- JS

    ```javascript
    try
    {
        const response = await $b24.callMethod(
            'bizproc.task.delegate',
            {
                TASK_IDS: [1128, 1129, 1130],
                FROM_USER_ID: 15,
                TO_USER_ID: 37,
            }
        );
        
        const result = response.getData().result;
        console.log('Delegated tasks:', result);
        
        processResult(result);
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
                'bizproc.task.delegate',
                [
                    'TASK_IDS' => [1128, 1129, 1130],
                    'FROM_USER_ID' => 15,
                    'TO_USER_ID' => 37,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error delegating task: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'bizproc.task.delegate',
        {
            'TASK_IDS': [1128, 1129, 1130],
            'FROM_USER_ID': 15,
            'TO_USER_ID': 37,
        },
        function(result)
        {
            if(result.error())
                alert("Error: " + result.error());
            else
                console.log(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'bizproc.task.delegate',
        [
            'TASK_IDS' => [1128, 1129, 1130],
            'FROM_USER_ID' => 15,
            'TO_USER_ID' => 37
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response handling
 
HTTP status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1748526089.625516,
        "finish": 1748526089.656787,
        "duration": 0.03127098083496094,
        "processing": 0.008746147155761719,
        "date_start": "2025-05-29T16:41:29+02:00",
        "date_finish": "2025-05-29T16:41:29+02:00",
        "operating_reset_at": 1748526689,
        "operating": 0
    }
}
```

### Returned data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Returns `true` if the task was successfully delegated. ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time. ||
|#

## Error handling

HTTP status: **400**

```json
{
    "error": "ERROR_INVALID_USER_ID",
    "error_description": "Invalid FROM_USER_ID"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible error codes
 
#|
|| **Code** | **Error message** | **Description** ||
|| `ERROR_TASK_VALIDATION` | Invalid TASK_IDS | Invalid task identifiers or the `TASK_IDS` parameter was not provided. ||
|| `ERROR_INVALID_USER_ID` | Invalid FROM_USER_ID | Invalid or missing user identifier from whom the delegation is made. ||
|| `ERROR_INVALID_USER_ID` | Invalid TO_USER_ID | Invalid or missing user identifier to whom the delegation is made. ||
|| `ERROR_DELEGATION_NOT_ALLOWED` | User is not responsible for the task | The user specified in the `FROM_USER_ID` parameter is not responsible for the task. ||
|| `ERROR_DELEGATION_NOT_ALLOWED` | Task delegation is only available for intranet users | The user specified in the `TO_USER_ID` parameter is not an intranet user. ||
|| `ERROR_DELEGATION_NOT_ALLOWED` | List of errors separated by `;` | The method can accept multiple tasks. If errors occur in multiple tasks, they will be returned as a list separated by `;`. ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue exploring 

- [{#T}](./index.md)
- [{#T}](./bizproc-task-list.md)
- [{#T}](./bizproc-task-complete.md)