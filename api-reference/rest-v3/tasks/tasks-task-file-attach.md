# Attach Files to a Task tasks.task.file.attach

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`tasks`](../../scopes/permissions.md)
>
> Who can execute the method: task Creator or a user with edit access to the task

The method `tasks.task.file.attach` adds files from Drive to a task. The user must have read access to the file or higher.

## Method Parameters

{% include [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **taskId***
[`integer`](../../data-types.md) | The identifier of the task to which files need to be attached.

The task identifier can be obtained when [creating a new task](./tasks-task-add.md) or by using the old method of [getting the task list](../../tasks/tasks-task-list.md) ||
|| **fileIds***
[`array<integer>`](../../data-types.md) | An array of file identifiers from Drive.

File identifiers can be obtained in two ways.

Use one of the file upload methods:
  - [disk.storage.uploadfile](../../disk/storage/disk-storage-upload-file.md)
  - [disk.folder.uploadfile](../../disk/folder/disk-folder-upload-file.md)

Use one of the methods to get the list of files:
  - [disk.storage.getchildren](../../disk/storage/disk-storage-get-children.md)
  - [disk.folder.getchildren](../../disk/folder/disk-folder-get-children.md) ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/tasks.task.file.attach`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"taskId":8017,"fileIds":[1065,1077]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/tasks.task.file.attach
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"taskId":8017,"fileIds":[1065,1077],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/tasks.task.file.attach
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type AttachFileResult = {
      result: boolean
    }

    try {
      const response = await $b24.actions.v3.call.make<AttachFileResult>({
        method: 'tasks.task.file.attach',
        params: {
          taskId: 8017,
          fileIds: [1065, 1077],
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Files attached successfully:', result.result)
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
      async function attachTaskFiles() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'tasks.task.file.attach',
            params: {
              taskId: 8017,
              fileIds: [1065, 1077],
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Files attached successfully:', result.result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', attachTaskFiles)
    </script>
    ```

- PHP

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'tasks.task.file.attach',
                [
                    'taskId' => 8017,
                    'fileIds' => [1065, 1077]
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error attaching file: ' . $e->getMessage();
    }
    ```

- BX24.js

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```js
    BX24.callMethod(
        'tasks.task.file.attach',
        {
            taskId: 8017,
            fileIds: [1065, 1077]
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
        'tasks.task.file.attach',
        [
            'taskId' => 8017,
            'fileIds' => [1065, 1077]
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
        "start": 1765357239,
        "finish": 1765357239.724951,
        "duration": 0.7249510288238525,
        "processing": 0,
        "date_start": "2025-12-10T12:00:39+01:00",
        "date_finish": "2025-12-10T12:00:39+01:00",
        "operating_reset_at": 1765357839,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | The root element of the response.

Contains an object with the key `result` and the value `true` if the file was successfully attached ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION",
        "message": "Error validating the request object",
        "validation": [
            {
                "message": "The required field `taskId` is missing",
                "field": "taskId"
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
|| `taskId`
`fileIds` | The required field `#FIELD#` is missing | Add the specified field to the request body ||
|| `#FIELD#` | The field `#FIELD#` requires data type `#TYPE#` for this request | Ensure the provided value is of the correct type ||
|| — | Insufficient permissions | No access to the specified file or task ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./tasks-task-update.md)
- [{#T}](./tasks-task-chat-message-send.md)
- [{#T}](./tasks-task-delete.md)
