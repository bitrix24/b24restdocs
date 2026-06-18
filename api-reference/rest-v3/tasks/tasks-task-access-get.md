# Check Access Permissions tasks.task.access.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`tasks`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `tasks.task.access.get` checks the available actions a user can perform on a task.

## Method Parameters

{% include [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Task identifier.

The task identifier can be obtained when [creating a new task](./tasks-task-add.md) or through the old method of [getting the task list](../../tasks/tasks-task-list.md) ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/tasks.task.access.get`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":8017}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/tasks.task.access.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":8017,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/tasks.task.access.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type TaskAccessResult = {
      read: boolean
      watch: boolean
      mute: boolean
      createSubtask: boolean
      createResult: boolean
      edit: boolean
      remove: boolean
      complete: boolean
      approve: boolean
      disapprove: boolean
      start: boolean
      take: boolean
      delegate: boolean
      defer: boolean
      renew: boolean
      deadline: boolean
      datePlan: boolean
      changeDirector: boolean
      changeResponsible: boolean
      changeAccomplices: boolean
      pause: boolean
      timeTracking: boolean
      mark: boolean
      changeStatus: boolean
      reminder: boolean
      addAuditors: boolean
      elapsedTime: boolean
      favorite: boolean
      checklistAdd: boolean
      checklistEdit: boolean
      checklistSave: boolean
      checklistToggle: boolean
      automate: boolean
      resultEdit: boolean
      completeResult: boolean
      removeResult: boolean
      resultRead: boolean
      admin: boolean
      copy: boolean
      saveAsTemplate: boolean
      attachFile: boolean
      detachFile: boolean
      detachParent: boolean
      createGanttDependence: boolean
      sort: boolean
    }

    try {
      const response = await $b24.actions.v3.call.make<TaskAccessResult>({
        method: 'tasks.task.access.get',
        params: {
          id: 8017,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Task access rights — edit:', result.edit, 'complete:', result.complete, 'remove:', result.remove)
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
      async function getTaskAccess() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'tasks.task.access.get',
            params: {
              id: 8017,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Task access rights — edit:', result.edit, 'complete:', result.complete, 'remove:', result.remove)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getTaskAccess)
    </script>
    ```

- PHP

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'tasks.task.access.get',
                [
                    'id' => 8017,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```js
    BX24.callMethod(
        'tasks.task.access.get',
        {
            id: 8017,
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
        'tasks.task.access.get',
        [
            'id' => 8017,
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
        "read": true,
        "watch": true,
        "mute": true,
        "createSubtask": true,
        "createResult": true,
        "edit": true,
        "remove": true,
        "complete": true,
        "approve": false,
        "disapprove": false,
        "start": false,
        "take": false,
        "delegate": true,
        "defer": false,
        "renew": false,
        "deadline": true,
        "datePlan": true,
        "changeDirector": false,
        "changeResponsible": true,
        "changeAccomplices": true,
        "pause": false,
        "timeTracking": false,
        "mark": true,
        "changeStatus": true,
        "reminder": true,
        "addAuditors": true,
        "elapsedTime": true,
        "favorite": true,
        "checklistAdd": true,
        "checklistEdit": true,
        "checklistSave": true,
        "checklistToggle": true,
        "automate": true,
        "resultEdit": false,
        "completeResult": true,
        "removeResult": false,
        "resultRead": false,
        "admin": true,
        "copy": true,
        "saveAsTemplate": true,
        "attachFile": true,
        "detachFile": true,
        "detachParent": true,
        "createGanttDependence": true,
        "sort": false
    },
    "time": {
        "start": 1764849882,
        "finish": 1764849882.731575,
        "duration": 0.7315750122070312,
        "processing": 0,
        "date_start": "2025-12-04T15:04:42+01:00",
        "date_finish": "2025-12-04T15:04:42+01:00",
        "operating_reset_at": 1764850482,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | The root element of the response. Contains an object describing the available actions for the current user ||
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
                "message": "Required field `id` is missing",
                "field": "id"
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
|| `id` | Required field `id` is missing | Add `id` to the request body ||
|| `id` | Field `id` requires data type `int` for this request | Ensure the value is a number, not a string ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./tasks-task-update.md)
- [{#T}](./tasks-task-chat-message-send.md)
- [{#T}](./tasks-task-delete.md)
