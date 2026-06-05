# Attach Files to a Task tasks.task.files.attach

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`task`](../scopes/permissions.md)
>
> Who can execute the method: task Creator or a user with access permission to edit the task.

The method `tasks.task.files.attach` adds a file from Drive to a task. The user must have read access to the file or higher.

## Method Parameters

{% include [Note on parameters](../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **taskId*** 
[`integer`](../data-types.md) | The identifier of the task to which the file needs to be attached.

The task identifier can be obtained when [creating a new task](./tasks-task-add.md) or by using the [get task list method](./tasks-task-list.md).
|| 
|| **fileId*** 
[`integer`](../data-types.md) | The identifier of the file on Drive.

You can obtain the file identifier in two ways.

Use one of the file upload methods:
  - [disk.storage.uploadfile](../disk/storage/disk-storage-upload-file.md)
  - [disk.folder.uploadfile](../disk/folder/disk-folder-upload-file.md)

Use one of the methods to get the list of files:
  - [disk.storage.getchildren](../disk/storage/disk-storage-get-children.md)
  - [disk.folder.getchildren](../disk/folder/disk-folder-get-children.md) ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"taskId":8017,"fileId":1065}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/tasks.task.files.attach
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"taskId":8017,"fileId":1065,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/tasks.task.files.attach
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
      attachmentId: number
    }

    try {
      const response = await $b24.actions.v2.call.make<AttachFileResult>({
        method: 'tasks.task.files.attach',
        params: {
          taskId: 8017,
          fileId: 1065,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Attachment ID:', result.attachmentId)
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
      async function attachFile() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'tasks.task.files.attach',
            params: {
              taskId: 8017,
              fileId: 1065,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Attachment ID:', result.attachmentId)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', attachFile)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'tasks.task.files.attach',
                [
                    'taskId' => 8017,
                    'fileId' => 1065
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

    ```js
    BX24.callMethod(
        'tasks.task.files.attach',
        {
            taskId: 8017,
            fileId: 1065
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
        'tasks.task.files.attach',
        [
            'taskId' => 8017,
            'fileId' => 1065
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
        "attachmentId": 1079
    },
    "time": {
        "start": 1758806783,
        "finish": 1758806783.609955,
        "duration": 0.6099550724029541,
        "processing": 0,
        "date_start": "2025-09-25T16:26:23+02:00",
        "date_finish": "2025-09-25T16:26:23+02:00",
        "operating_reset_at": 1758807383,
        "operating": 0.4156019687652588
    }
}
```

### Returned Data

#| 
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | The root element of the response. Contains an object describing the attached file ||
|| **attachmentId**
[`integer`](../data-types.md) | The identifier of the file attachment to the task.

You can get file data by the attachment identifier using the [disk.attachedObject.get](../disk/attached-object/disk-attached-object-get.md) method ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "100",
    "error_description": "Could not find value for parameter {fileId} (internal error)"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#| 
|| **Code** | **Description** | **Value** ||
|| `100` | CTaskItem All parameters in the constructor must have real class type (internal error) | Required parameter `taskId` is not specified ||
|| `0` | wrong task id (internal error) | The value of `taskId` is of an incorrect type ||
|| `100` | Could not find value for parameter \{fileId\} (internal error) | Required parameter `fileId` is not specified ||
|| `100` | Invalid value {value} to match with parameter \{fileId\}. Should be value of type int. (internal error) | The value of `fileId` is of an incorrect type ||
|| `ERROR_CORE` | Insufficient permissions.\\u003Cbr\\u003E | No access to the specified file ||
|| `0` | Access denied (internal error) | Insufficient permissions to modify the task ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](../../tutorials/tasks/how-to-upload-file-to-task.md)
- [{#T}](./tasks-task-get.md)