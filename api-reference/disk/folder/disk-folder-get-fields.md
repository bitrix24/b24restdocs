# Get Folder Field Descriptions disk.folder.getfields

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `disk.folder.getfields` returns the description of folder fields.

## Method Parameters

No parameters.

## Code Examples

{% include [Examples Note](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/disk.folder.getfields
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/disk.folder.getfields
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type FolderField = {
      TYPE: string
      USE_IN_FILTER: boolean
      USE_IN_SHOW: boolean
    }

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type FolderFieldsResult = Record<string, FolderField>

    try {
      const response = await $b24.actions.v2.call.make<FolderFieldsResult>({
        method: 'disk.folder.getfields',
        params: {},
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(Object.keys(result), result['NAME'])
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
      async function getFolderFields() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'disk.folder.getfields',
            params: {},
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(Object.keys(result), result['NAME'])
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getFolderFields)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'disk.folder.getfields',
                []
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "disk.folder.getfields",
        {},
        function (result) {
            if (result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'disk.folder.getfields',
        []
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
        "ID": {
            "TYPE": "integer",
            "USE_IN_FILTER": true,
            "USE_IN_SHOW": true
        },
        "NAME": {
            "TYPE": "string",
            "USE_IN_FILTER": true,
            "USE_IN_SHOW": true
        },
        "TYPE": {
            "TYPE": "enum",
            "USE_IN_FILTER": true,
            "USE_IN_SHOW": true
        },
        "CODE": {
            "TYPE": "string",
            "USE_IN_FILTER": true,
            "USE_IN_SHOW": true
        },
        "STORAGE_ID": {
            "TYPE": "integer",
            "USE_IN_FILTER": true,
            "USE_IN_SHOW": true
        },
        "REAL_OBJECT_ID": {
            "TYPE": "integer",
            "USE_IN_FILTER": false,
            "USE_IN_SHOW": true
        },
        "PARENT_ID": {
            "TYPE": "integer",
            "USE_IN_FILTER": true,
            "USE_IN_SHOW": true
        },
        "CREATE_TIME": {
            "TYPE": "datetime",
            "USE_IN_FILTER": true,
            "USE_IN_SHOW": true
        },
        "UPDATE_TIME": {
            "TYPE": "datetime",
            "USE_IN_FILTER": true,
            "USE_IN_SHOW": true
        },
        "DELETE_TIME": {
            "TYPE": "datetime",
            "USE_IN_FILTER": true,
            "USE_IN_SHOW": true
        },
        "CREATED_BY": {
            "TYPE": "integer",
            "USE_IN_FILTER": false,
            "USE_IN_SHOW": true
        },
        "UPDATED_BY": {
            "TYPE": "integer",
            "USE_IN_FILTER": false,
            "USE_IN_SHOW": true
        },
        "DELETED_BY": {
            "TYPE": "integer",
            "USE_IN_FILTER": false,
            "USE_IN_SHOW": true
        },
        "DELETED_TYPE": {
            "TYPE": "enum",
            "USE_IN_FILTER": true,
            "USE_IN_SHOW": true
        }
    },
    "time": {
        "start": 1768343226,
        "finish": 1768343226.742822,
        "duration": 0.7428219318389893,
        "processing": 0,
        "date_start": "2026-01-13T16:47:06+01:00",
        "date_finish": "2026-01-13T16:47:06+01:00",
        "operating_reset_at": 1768343826,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | Array with the description of folder fields.

The structure of each field description:
- `TYPE` — data type of the field
- `USE_IN_FILTER` — ability to use the field for filtering
- `USE_IN_SHOW` — availability of the field when receiving a response ||
|| **ID**
[`integer`](../../data-types.md) | Folder identifier ||
|| **NAME**
[`string`](../../data-types.md) | Folder name ||
|| **TYPE**
[`enum`](../../data-types.md) | Object type ||
|| **CODE**
[`string`](../../data-types.md) | Symbolic code of the folder ||
|| **STORAGE_ID**
[`integer`](../../data-types.md) | Identifier of the storage where the folder is located ||
|| **REAL_OBJECT_ID**
[`integer`](../../data-types.md) | Object identifier ||
|| **PARENT_ID**
[`integer`](../../data-types.md) | Identifier of the parent folder ||
|| **CREATE_TIME**
[`datetime`](../../data-types.md) | Date and time of folder creation ||
|| **UPDATE_TIME**
[`datetime`](../../data-types.md) | Date and time of the last update of the folder ||
|| **DELETE_TIME**
[`datetime`](../../data-types.md) | Date and time of moving the folder to the trash ||
|| **CREATED_BY**
[`integer`](../../data-types.md) | Identifier of the user who created the folder ||
|| **UPDATED_BY**
[`integer`](../../data-types.md) | Identifier of the user who made the last change ||
|| **DELETED_BY**
[`integer`](../../data-types.md) | Identifier of the user who deleted the folder ||
|| **DELETED_TYPE**
[`enum`](../../data-types.md) | Deletion status of the object ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./disk-folder-add-subfolder.md)
- [{#T}](./disk-folder-copy-to.md)
- [{#T}](./disk-folder-delete-tree.md)
- [{#T}](./disk-folder-get-children.md)
- [{#T}](./disk-folder-get-external-link.md)
- [{#T}](./disk-folder-get.md)
- [{#T}](./disk-folder-mark-deleted.md)
- [{#T}](./disk-folder-move-to.md)
- [{#T}](./disk-folder-rename.md)
- [{#T}](./disk-folder-restore.md)
- [{#T}](./disk-folder-share-to-user.md)
- [{#T}](./disk-folder-upload-file.md)