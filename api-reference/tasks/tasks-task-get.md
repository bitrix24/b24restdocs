# Get Task by ID tasks.task.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`task`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `tasks.task.get` returns information about a task by its ID.

Access to the data depends on permissions:
- Administrators can see all tasks,
- Managers can see their employees' tasks,
- Others can only see tasks available to them.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **taskId**
[`integer`](../data-types.md) | Task ID. 

The task ID can be obtained when [creating a new task](./tasks-task-add.md) or by using the [get task list](./tasks-task-list.md) method. ||
|| **select**
[`array`](../data-types.md) | An array of record fields that will be returned by the method. You can specify only the fields you need. If the array contains the value `"*"`, all available fields will be returned. 

By default, it returns all fields except for custom ones. It is recommended to specify particular fields in the selection, as default fields may change.

System fields `UF_CRM_TASK`, `UF_TASK_WEBDAV_FILES`, and `UF_MAIL_MESSAGE` are not returned by default. Specify one of these fields in `SELECT` to get their values. 

To retrieve custom fields, specify them in `SELECT`. You can find the names of custom fields using the [tasks.task.getFields](./tasks-task-get-fields.md) method. ||
|#

{% note info "" %}

The `CHAT_ID` field, which is the chat ID for the [new task card](tasks-new.md), is returned by default. Use its value to work with [chat](../chats/index.md) methods.

{% endnote %}

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

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // result.task limited to the requested (select) fields, in the SDK camelCase form
    // Shape of the payload returned in result (match the "response handling" section of the page)
    type TaskGetResult = {
      task: {
        id: string
        title: string
        description: string
        createdBy: string
        responsibleId: string
        deadline: ISODate | null
        ufCrmTask: string[]
        ufTaskWebdavFiles: number[]
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<TaskGetResult>({
        method: 'tasks.task.get',
        params: {
          taskId: 8017, // ID of the task to read
          // Request only the fields you need
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
        requestId: Text.getUuidRfc4122() // optional unique tracking id for this request
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const task = response.getData()!.result.task
        console.info(`Fetched task ${task.id}: ${task.title}`)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function getTask() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'tasks.task.get',
            params: {
              taskId: 8017, // ID of the task to read
              // Request only the fields you need
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
            requestId: B24Js.Text.getUuidRfc4122() // optional unique tracking id for this request
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const task = response.getData().result.task
          console.info(`Fetched task ${task.id}: ${task.title}`)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getTask)
    </script>
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

HTTP Status: **200**

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

Returns an empty array `"result":[],` if the task does not exist or the user does not have access to the task. ||
|| **task**
[`object`](../data-types.md) | Object with [task description](./fields.md) after the operation is performed. ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time. ||
|#

## Error Handling

HTTP Status: **400**

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
|| `0` | wrong task id | The value of the `taskId` parameter is of an incorrect type. ||
|| `100` | CTaskItem All parameters in the constructor must have real class type (internal error) | The required parameter `taskId` is missing. ||
|| `100` | Invalid value {} to match with parameter {select}. Should be value of type array. (internal error) | The `select` parameter is empty or contains invalid values. ||
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