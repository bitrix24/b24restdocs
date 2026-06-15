# Copy File to Specified Folder disk.file.copyto

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: a user with "Read" permission for the file and "Add" permission for the folder

The method `disk.file.copyto` copies a file to the specified folder.

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Identifier of the file ||
|| **targetFolderId***
[`integer`](../../data-types.md) | Identifier of the folder to which the file is copied ||
|#

{% note info "" %}

File and folder identifiers can be obtained using the methods [disk.storage.getchildren](../storage/disk-storage-get-children.md) and [disk.folder.getchildren](../folder/disk-folder-get-children.md)

{% endnote %}

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":9035,"targetFolderId":8930}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/disk.file.copyto
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":9035,"targetFolderId":8930,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/disk.file.copyto
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type DiskFileCopyToResult = {
      ID: number
      NAME: string
      CODE: string | null
      STORAGE_ID: string
      TYPE: string
      PARENT_ID: string
      DELETED_TYPE: number
      GLOBAL_CONTENT_VERSION: number
      FILE_ID: number
      SIZE: string
      CREATE_TIME: ISODate | null
      UPDATE_TIME: ISODate | null
      DELETE_TIME: ISODate | null
      CREATED_BY: string
      UPDATED_BY: string
      DELETED_BY: string | null
      DOWNLOAD_URL: string
      DETAIL_URL: string
    }

    try {
      const response = await $b24.actions.v2.call.make<DiskFileCopyToResult>({
        method: 'disk.file.copyto',
        params: {
          id: 9035,
          targetFolderId: 8930,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Copied file:', result.ID, result.NAME, result.DOWNLOAD_URL)
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
      async function copyFileTo() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'disk.file.copyto',
            params: {
              id: 9035,
              targetFolderId: 8930,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Copied file:', result.ID, result.NAME, result.DOWNLOAD_URL)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', copyFileTo)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'disk.file.copyto',
                [
                    'id' => 9035,
                    'targetFolderId' => 8930
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error copying file: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "disk.file.copyto",
        {
            id: 9035,
            targetFolderId: 8930
        },
        function (result)
        {
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
        'disk.file.copyto',
        [
            'id' => 9035,
            'targetFolderId' => 8930
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
        "ID": 9037,
        "NAME": "picture.png",
        "CODE": null,
        "STORAGE_ID": "1357",
        "TYPE": "file",
        "PARENT_ID": "8930",
        "DELETED_TYPE": 0,
        "GLOBAL_CONTENT_VERSION": 1,
        "FILE_ID": 32933,
        "SIZE": "1679",
        "CREATE_TIME": "2026-02-09T11:35:49+02:00",
        "UPDATE_TIME": "2026-02-09T11:35:49+02:00",
        "DELETE_TIME": null,
        "CREATED_BY": "1269",
        "UPDATED_BY": "1269",
        "DELETED_BY": null,
        "DOWNLOAD_URL": "https://test.bitrix24.com/rest/download.json?auth=effc89690000071b006e2cf2000004f50000077d3d5d904ee6ccfc65bf287ca71f1fd6&token=disk%7CaWQ9OTAzNyZfPVhsRFgwaWJ2RTdLMXJlV1dhaEFPMEtoTjhVQ0s0MWNx%7CImRvd25sb2FkfGRpc2t8YVdROU9UQXpOeVpmUFZoc1JGZ3dhV0oyUlRkTE1YSmxWMWRoYUVGUE1FdG9UamhWUTBzME1XTnh8ZWZmYzg5NjkwMDAwMDcxYjAwNmUyY2YyMDAwMDA0ZjUwMDAwMDc3ZDNkNWQ5MDRlZTZjY2ZjNjViZjI4N2NhNzFmMWZkNiI%3D.fY5cpLbXwIiIO8X3NoiCzsMVAl2i6zegF4%2Bn86l0khg%3D",
        "DETAIL_URL": "https://test.bitrix24.com/company/personal/user/1269/disk/file/Folder/Folder in folder/picture.png"
    },
    "time": {
        "start": 1770647749,
        "finish": 1770647749.342314,
        "duration": 0.3423140048980713,
        "processing": 0,
        "date_start": "2026-02-09T11:35:49+02:00",
        "date_finish": "2026-02-09T11:35:49+02:00",
        "operating_reset_at": 1770648349,
        "operating": 0.1015939712524414
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | Array with file fields ||
|| **ID**
[`integer`](../../data-types.md) | Identifier of the file ||
|| **NAME**
[`string`](../../data-types.md) | Name of the file ||
|| **CODE**
[`string`](../../data-types.md) | Symbolic code of the file ||
|| **STORAGE_ID**
[`integer`](../../data-types.md) | Identifier of the storage where the file is located ||
|| **TYPE**
[`enum`](../../data-types.md) | Type of the object ||
|| **PARENT_ID**
[`integer`](../../data-types.md) | Identifier of the parent folder ||
|| **DELETED_TYPE**
[`enum`](../../data-types.md) | Deletion status of the object. Possible values:
- `0` — not deleted
- `3` — in the trash
- `4` — deleted along with the parent folder ||
|| **GLOBAL_CONTENT_VERSION**
[`integer`](../../data-types.md) | Incremental version counter of the file ||
|| **FILE_ID**
[`integer`](../../data-types.md) | Internal value of the file identifier ||
|| **SIZE**
[`integer`](../../data-types.md) | Size of the file in bytes ||
|| **CREATE_TIME**
[`datetime`](../../data-types.md) | Date and time of file creation ||
|| **UPDATE_TIME**
[`datetime`](../../data-types.md) | Date and time of the last update of the file ||
|| **DELETE_TIME**
[`datetime`](../../data-types.md) | Date and time the file was moved to the trash ||
|| **CREATED_BY**
[`integer`](../../data-types.md) | Identifier of the user who created the file ||
|| **UPDATED_BY**
[`integer`](../../data-types.md) | Identifier of the user who made the last change ||
|| **DELETED_BY**
[`integer`](../../data-types.md) | Identifier of the user who deleted the file ||
|| **DOWNLOAD_URL**
[`string`](../../data-types.md) | Link to download the file ||
|| **DETAIL_URL**
[`string`](../../data-types.md) | Link to open the file in the interface ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error":"ERROR_ARGUMENT",
    "error_description":"Invalid value of parameter {Parameter #0}"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ERROR_ARGUMENT` | Invalid value of parameter {Parameter #0} | Required parameter `id` or `targetFolderId` is missing ||
|| `DISK_OBJ_22000` | A file with this name already exists | A file with this name already exists ||
|| `DISK_FILE_22003` | Could not copy file | Error creating a copy of the file in the database ||
|| `ERROR_NOT_FOUND` | Could not find entity with id `X` | File with the specified `id` or folder with the specified `targetFolderId` not found ||
|| `ACCESS_DENIED` | Access denied | Insufficient permissions to copy the file ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./disk-file-delete.md)
- [{#T}](./disk-file-get-external-link.md)
- [{#T}](./disk-file-get-fields.md)
- [{#T}](./disk-file-get-versions.md)
- [{#T}](./disk-file-get.md)
- [{#T}](./disk-file-mark-deleted.md)
- [{#T}](./disk-file-move-to.md)
- [{#T}](./disk-file-rename.md)
- [{#T}](./disk-file-restore-from-version.md)
- [{#T}](./disk-file-restore.md)
- [{#T}](./disk-file-upload-version.md)