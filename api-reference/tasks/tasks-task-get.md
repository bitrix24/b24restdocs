# Get Task by ID tasks.task.get

> Scope: [`task`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `tasks.task.get` returns information about a task by its ID.

Access to the data depends on permissions:
- administrators see all tasks,
- managers see their employees' tasks,
- others see only the tasks available to them.

{% note info "" %}

Since version `tasks 25.700.0`, the method call is available in two API versions.

Old version:

`https://{installation_address}/rest/{user_id}/{rest_application_password}/tasks.task.get`

New version:

`https://{installation_address}/rest/api/{user_id}/{rest_application_password}/tasks.task.get`

Documentation in OpenApi format is available for the new version of the method call. To obtain OpenApi, call the method `documentation`:

`https://{installation_address}/rest/api/{user_id}/{rest_application_password}/documentation`

{% endnote %}

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **taskId**
[`integer`](../data-types.md) | Task identifier.

The task identifier can be obtained when [creating a new task](./tasks-task-add.md) or by using the [get task list method](./tasks-task-list.md) ||
|| **select**
[`array`](../data-types.md) | Array of record fields that will be returned by the method. You can specify only the fields that are necessary. If the array contains the value `"*"`, all available fields will be returned.

By default, it returns all fields except for custom ones. It is recommended to specify specific fields in the selection, as default fields may change.

To obtain system `UF_CRM_TASK`, `UF_TASK_WEBDAV_FILES`, `UF_MAIL_MESSAGE`, and custom fields, specify them in `SELECT`. You can find the names of custom fields using the [tasks.task.getFields](./tasks-task-get-fields.md) method.

Specify `CHAT_ID` in select to get the chat ID for the [new task card](tasks-new.md) ||
|#

## Code Examples

{% include [Examples Note](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"taskId":8017,"select":["ID","TITLE","DESCRIPTION","CREATED_BY","RESPONSIBLE_ID","DEADLINE","UF_CRM_TASK","UF_TASK_WEBDAV_FILES"]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/tasks.task.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"taskId":8017,"select":["ID","TITLE","DESCRIPTION","CREATED_BY","RESPONSIBLE_ID","DEADLINE","UF_CRM_TASK","UF_TASK_WEBDAV_FILES"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/tasks.task.get
    ```

- JS

    ```javascript
    try
    {
        const response = await $b24.callMethod(
            'tasks.task.get',
            {
                taskId: 8017,
                select: [
                    'ID',
                    'TITLE',
                    'DESCRIPTION',
                    'CREATED_BY',
                    'RESPONSIBLE_ID',
                    'DEADLINE',
                    'UF_CRM_TASK',
                    'UF_TASK_WEBDAV_FILES'
                ]
            }
        );
        
        const result = response.getData().result;
        console.log('Fetched task:', result);
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
                'tasks.task.get',
                [
                    'taskId' => 8017,
                    'select' => [
                        'ID',
                        'TITLE',
                        'DESCRIPTION',
                        'CREATED_BY',
                        'RESPONSIBLE_ID',
                        'DEADLINE',
                        'UF_CRM_TASK',
                        'UF_TASK_WEBDAV_FILES'
                    ]
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching task: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'tasks.task.get',
        {
            taskId: 8017,
            select: [
                'ID',
                'TITLE',
                'DESCRIPTION',
                'CREATED_BY',
                'RESPONSIBLE_ID',
                'DEADLINE',
                'UF_CRM_TASK',
                'UF_TASK_WEBDAV_FILES'
            ]
        },
        function(result){
            console.info(result.data());
            console.log(result);
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'tasks.task.get',
        [
            'taskId' => 8017,
            'select' => [
                'ID',
                'TITLE',
                'DESCRIPTION',
                'CREATED_BY',
                'RESPONSIBLE_ID',
                'DEADLINE',
                'UF_CRM_TASK',
                'UF_TASK_WEBDAV_FILES'
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
        "task": {
            "id": "8017",
            "title": "Task Example",
            "description": "Task description with [B]formatting[/B]",
            "createdBy": "503",
            "responsibleId": "547",
            "deadline": "2025-10-24T19:00:00+02:00",
            "ufCrmTask": ["C_627", "CO_591", "L_1177", "T88_3", "D_1723"],
            "ufTaskWebdavFiles": [1065, 1077],
            "ufMailMessage": null,
            "descriptionInBbcode": "Y",
            "favorite": "Y",
            "group": [],
            "creator": {
                "id": "503",
                "name": "Maria Johnson",
                "link": "/company/personal/user/503/",
                "icon": "https://mysite.com/b17053/resize_cache/45749/c0120a8d7c10d63c83e32398d1ec4d9e/main/c89/c89c6b7301880958ea704b5a8470635c/4R5A1256.png",
                "workPosition": "admin"
            },
            "responsible": {
                "id": "547",
                "name": "Maria",
                "link": "/company/personal/user/547/",
                "icon": "/bitrix/images/tasks/default_avatar.png",
                "workPosition": "Tester"
            },
            "action": {
                "accept": false,
                "decline": false,
                "complete": true,
                "approve": false,
                "disapprove": false,
                "start": true,
                "pause": false,
                "delegate": true,
                "remove": true,
                "edit": true,
                "defer": true,
                "renew": false,
                "create": true,
                "changeDeadline": true,
                "checklistAddItems": true,
                "addFavorite": false,
                "deleteFavorite": true,
                "rate": true,
                "take": false,
                "edit.originator": false,
                "checklist.reorder": true,
                "elapsedtime.add": true,
                "dayplan.timer.toggle": true,
                "edit.plan": true,
                "checklist.add": true,
                "favorite.add": false,
                "favorite.delete": true
            }
        }
    },
    "time": {
        "start": 1759759363,
        "finish": 1759759363.155413,
        "duration": 0.15541291236877441,
        "processing": 0,
        "date_start": "2025-10-06T17:02:43+02:00",
        "date_finish": "2025-10-06T17:02:43+02:00",
        "operating_reset_at": 1759759963,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | Object with response data.

Returns an empty array `"result":[],` if the task does not exist or the user does not have access to the task ||
|| **task**
[`object`](../data-types.md) | Object with [task description](./fields.md) after the operation is performed ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "100",
    "error_description": "Invalid value {} to match with parameter {select}. Should be value of type array. (internal error)"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `0` | wrong task id | The value of the `taskId` parameter is of an incorrect type ||
|| `100` | CTaskItem All parameters in the constructor must have real class type (internal error) | The required parameter `taskId` was not provided ||
|| `100` | Invalid value {} to match with parameter {select}. Should be value of type array. (internal error) | The `select` parameter was passed empty or contains invalid values ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./tasks-task-add.md)
- [{#T}](./tasks-task-update.md)
- [{#T}](./tasks-task-list.md)
- [{#T}](./tasks-task-delete.md)
- [{#T}](./tasks-task-get-fields.md)
- [{#T}](../../tutorials/tasks/how-to-create-task-with-file.md)
- [{#T}](../../tutorials/tasks/how-to-connect-task-to-spa.md)