# Get Information About Attached File disk.attachedObject.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: a user with "Read" access permission for the required file

The method `disk.attachedObject.get` returns information about the attached file.

## Method Parameters

{% include [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Identifier of the file attachment record, which is the link connecting the disk file [with other objects](../index.md#diskconnection).

To obtain the connection identifier, use methods that return attached files. For example, if the file is attached to a task, you can find out the connection identifier using the [tasks.task.get](../../tasks/tasks-task-get.md) method.
||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":495}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/disk.attachedObject.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":495,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/disk.attachedObject.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type AttachedObjectResult = {
      ID: string
      OBJECT_ID: string
      MODULE_ID: string
      ENTITY_TYPE: string
      ENTITY_ID: string
      CREATE_TIME: ISODate | null
      CREATED_BY: string
      DOWNLOAD_URL: string
      NAME: string
      SIZE: string
    }

    try {
      const response = await $b24.actions.v2.call.make<AttachedObjectResult>({
        method: 'disk.attachedObject.get',
        params: {
          id: 495,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Attached file:', result.NAME, 'size:', result.SIZE, 'entity:', result.ENTITY_TYPE)
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
      async function getAttachedObject() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'disk.attachedObject.get',
            params: {
              id: 495,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Attached file:', result.NAME, 'size:', result.SIZE, 'entity:', result.ENTITY_TYPE)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getAttachedObject)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'disk.attachedObject.get',
                [
                    'id' => 495
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error retrieving attached object: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'disk.attachedObject.get',
        {
            id: 495
        },
        function (result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'disk.attachedObject.get',
        [
            'id' => 495
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
        "ID": "495",
        "OBJECT_ID": "8903",
        "MODULE_ID": "tasks",
        "ENTITY_TYPE": "tasks_task",
        "ENTITY_ID": "3845",
        "CREATE_TIME": "2025-12-23T10:31:24+02:00",
        "CREATED_BY": "1269",
        "DOWNLOAD_URL": "https://test.bitrix24.com/bitrix/tools/disk/uf.php?attachedId=495&auth[auth]=d78a4a690000071b006e2cf2000004f5000007746b9ad166e1b9bd67b8848714afc5a7&action=download&ncc=1",
        "NAME": "Picture.png",
        "SIZE": "52486"
    },
    "time": {
        "start": 1766489404,
        "finish": 1766489404.720053,
        "duration": 0.72005295753479,
        "processing": 0,
        "date_start": "2025-12-23T11:30:04+02:00",
        "date_finish": "2025-12-23T11:30:04+02:00",
        "operating_reset_at": 1766490004,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Object containing data about the attached file ||
|| **ID**
[`integer`](../../data-types.md) | Identifier of the file attachment record ||
|| **OBJECT_ID**
[`integer`](../../data-types.md) | Identifier of the file ||
|| **MODULE_ID**
[`string`](../../data-types.md) | Module where the file is used ||
|| **ENTITY_TYPE**
[`string`](../../data-types.md) | Type of the linked object ||
|| **ENTITY_ID**
[`integer`](../../data-types.md) | Identifier of the item to which the file is attached ||
|| **CREATE_TIME**
[`string`](../../data-types.md) | Time of the attachment creation ||
|| **CREATED_BY**
[`integer`](../../data-types.md) | Identifier of the user who attached the file ||
|| **DOWNLOAD_URL**
[`string`](../../data-types.md) | Link to download the file ||
|| **NAME**
[`string`](../../data-types.md) | Name of the file ||
|| **SIZE**
[`integer`](../../data-types.md) | Size of the file in bytes ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error":"ERROR_NOT_FOUND",
    "error_description":"Could not find entity with id `X`"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ERROR_NOT_FOUND` | Could not find entity with id `X` | Attached file with the specified `id` not found ||
|| `ACCESS_DENIED` | Access denied | Insufficient rights to read the attached file ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](../../../tutorials/tasks/how-to-create-comment-with-file.md)
- [{#T}](../../../tutorials/tasks/how-to-create-task-with-file.md)
- [{#T}](../../../tutorials/tasks/how-to-upload-file-to-task.md)