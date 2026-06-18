# Update Task tasks.task.update

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`tasks`](../../scopes/permissions.md)
>
> Who can execute the method: task Creator or a user with access permission to edit the task

The method `tasks.task.update` updates a task.

## Method Parameters

{% include [Footnote about parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md)  | Task identifier.

The task identifier can be obtained when [creating a new task](./tasks-task-add.md) or by using the old method of [getting the task list](../../tasks/tasks-task-list.md) ||
|| **fields***
[`object`](../../data-types.md) | Values of the task fields to be updated.
[Description of all task fields](./fields.md) ||
|#

## Code Examples

{% include [Footnote about examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/tasks.task.update`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":11,"fields":{"title":"Task Title","deadline":"2025-12-31T23:59:59+02:00","creatorId":29,"responsibleId":1,"crmItemIds":["L_1000959"]}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/tasks.task.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":11,"fields":{"title":"Task Title","deadline":"2025-12-31T23:59:59+02:00","creatorId":29,"responsibleId":1,"crmItemIds":["L_1000959"]},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/tasks.task.update
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type TaskUpdateResult = {
      result: boolean
    }

    try {
      const response = await $b24.actions.v3.call.make<TaskUpdateResult>({
        method: 'tasks.task.update',
        params: {
          id: 11,
          fields: {
            title: 'Task title',
            deadline: '2025-12-31T23:59:59+02:00',
            creatorId: 29,
            responsibleId: 1,
            crmItemIds: ['L_1000959'],
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Task updated:', result.result)
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
      async function updateTask() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'tasks.task.update',
            params: {
              id: 11,
              fields: {
                title: 'Task title',
                deadline: '2025-12-31T23:59:59+02:00',
                creatorId: 29,
                responsibleId: 1,
                crmItemIds: ['L_1000959'],
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Task updated:', result.result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateTask)
    </script>
    ```

- PHP

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'tasks.task.update',
                [
                    'id' => 11,
                    'fields' => [
                        'title' => 'Task Title',
                        'deadline' => '2025-12-31T23:59:59+02:00',
                        'creatorId' => 29,
                        'responsibleId' => 1,
                        'crmItemIds' => ['L_1000959'],
                    ]
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating task: ' . $e->getMessage();
    }
    ```

- BX24.js

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```js
    BX24.callMethod(
        'tasks.task.update',
        {
            id: 11,
            fields: {
                title: 'Task Title',
                deadline: '2025-12-31T23:59:59+02:00',
                creatorId: 29,
                responsibleId: 1,
                crmItemIds: ['L_1000959']
            }
        },
        function(result){
            console.info(result.data());
            console.log(result);
        }
    );
    ```

- PHP CRest

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'tasks.task.update',
        [
            'id' => 11,
            'fields' => [
                'title' => 'Task Title',
                'deadline' => '2025-12-31T23:59:59+02:00',
                'creatorId' => 29,
                'responsibleId' => 1,
                'crmItemIds' => ['L_1000959']
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
        "result": true
    },
    "time": {
        "start": 1765452441,
        "finish": 1765452442.17818,
        "duration": 1.1781799793243408,
        "processing": 1,
        "date_start": "2025-12-11T14:27:21+01:00",
        "date_finish": "2025-12-11T14:27:22+01:00",
        "operating_reset_at": 1765453041,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Root element of the response.

Contains an object with the key `result` and the value `true` if the task was successfully updated ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION",
        "message": "Error during request object validation",
        "validation": [
            {
                "message": "In DTO `TaskDto`, the field `creatorId` requires the presence of the `Editable` attribute for such a request",
                "field": "creatorId"
            }
        ]
    }
}
```

{% include notitle [error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors  

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `id`
`fields` | Required field `#FIELD#` is missing | Add the specified field to the request body ||
|| `#FIELD#` | The field `#FIELD#` requires data type `#TYPE#` for such a request | Ensure the provided value is of the correct type ||
|| `responsibleId` | The user specified in the "Responsible" field was not found | Specify the identifier of an existing user in the `responsibleId` field ||
|| `parentId` | The task specified in the "Parent Task" field was not found | Specify the identifier of an existing task in the `parentId` field ||
|| `parentId` | Cannot bind a node to itself | Specify a task identifier in the `parentId` field that is different from the `id` field ||
|| `endPlan` | The specified end date is earlier than the start date in the planning | Specify an `endPlan` date that is later than `startPlan` ||
|| `endPlan` | The specified duration of the task is too long | Reduce the date in the `endPlan` field ||
|| `creatorId` | In DTO `TaskDto`, the field `creatorId` requires the presence of the `Editable` attribute for such a request | Remove the `creatorId` field from the request, it cannot be modified ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `id` | Access denied | No access to the task or the task does not exist ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./tasks-task-access-get.md)
- [{#T}](./tasks-task-chat-message-send.md)
- [{#T}](./tasks-task-delete.md)
