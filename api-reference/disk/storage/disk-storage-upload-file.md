# Upload a New File to the Root of the Storage disk.storage.uploadfile

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: a user with "Add" access permission for the required storage

The method `disk.storage.uploadfile` uploads a new file to the root of the storage.

## Method Parameters

{% include [Note on Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Identifier of the storage where the file needs to be uploaded.

The identifier can be obtained using the method [disk.storage.getlist](../storage/disk-storage-get-list.md)
||
|| **data***
[`array`](../../data-types.md) | An array with the field `NAME`, where `NAME` is the name of the file ||
|| **fileContent***
[`array`](../../data-types.md) | An array containing the file name and a string with [Base64](../../files/how-to-upload-files.md) ||
|| **rights**
[`array`](../../data-types.md) | An array of access permissions for the uploaded file in the format `{"TASK_ID": 42, "ACCESS_CODE": "U35"}`, where
- `TASK_ID` — identifier of the access level
- `ACCESS_CODE` — access code consisting of a letter code for the user or department and an identifier

User categories:
- `U` — user
- `*` — all users
- `D` — all department employees
- `DR` — all department employees including sub-departments

The list of available `TASK_ID` identifiers for setting permissions can be obtained using the method [disk.rights.getTasks](../rights/disk-rights-get-tasks.md) ||
|| **generateUniqueName**
[`boolean`](../../data-types.md) | Generate a unique file name if a file with that name already exists. For example, file (1).docx.

Possible values:
- `true` — generate
- `false` — do not generate

Default is `false` ||
|#

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1357,"data":{"NAME":"picture.png"},"fileContent":["picture.png","iVBORw0KGgoAAAANSUhEUgAAAD4AAABDCAYAAADEfbZbAAAACXBIWXMAABJ0AAASdAHeZh94...RK5CYII="],"generateUniqueName":true,"rights":[{"TASK_ID":79,"ACCESS_CODE":"U1271"}]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/disk.storage.uploadfile
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1357,"data":{"NAME":"picture.png"},"fileContent":["picture.png","iVBORw0KGgoAAAANSUhEUgAAAD4AAABDCAYAAADEfbZbAAAACXBIWXMAABJ0AAASdAHeZh94...RK5CYII="],"generateUniqueName":true,"rights":[{"TASK_ID":79,"ACCESS_CODE":"U1271"}],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/disk.storage.uploadfile
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type UploadFileResult = {
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
      CREATE_TIME: ISODate
      UPDATE_TIME: ISODate
      DELETE_TIME: ISODate | null
      CREATED_BY: string
      UPDATED_BY: string
      DELETED_BY: string | null
      DOWNLOAD_URL: string
      DETAIL_URL: string
    }

    try {
      const response = await $b24.actions.v2.call.make<UploadFileResult>({
        method: 'disk.storage.uploadfile',
        params: {
          id: 1357,
          data: {
            NAME: 'picture.png',
          },
          fileContent: [
            'picture.png',
            'iVBORw0KGgoAAAANSUhEUgAAAD4AAABDCAYAAADEfbZbAAAACXBIWXMAABJ0AAASdAHeZh94...RK5CYII=',
          ],
          generateUniqueName: true,
          rights: [
            {
              TASK_ID: 79,
              ACCESS_CODE: 'U1271',
            },
          ],
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.ID, result.NAME, result.DOWNLOAD_URL)
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
      async function uploadFile() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'disk.storage.uploadfile',
            params: {
              id: 1357,
              data: {
                NAME: 'picture.png',
              },
              fileContent: [
                'picture.png',
                'iVBORw0KGgoAAAANSUhEUgAAAD4AAABDCAYAAADEfbZbAAAACXBIWXMAABJ0AAASdAHeZh94...RK5CYII=',
              ],
              generateUniqueName: true,
              rights: [
                {
                  TASK_ID: 79,
                  ACCESS_CODE: 'U1271',
                },
              ],
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.ID, result.NAME, result.DOWNLOAD_URL)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', uploadFile)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'disk.storage.uploadfile',
                [
                    'id' => 1357,
                    'data' => [
                        'NAME' => 'picture.png'
                    ],
                    'fileContent' => [
                        'picture.png',
                        'iVBORw0KGgoAAAANSUhEUgAAAD4AAABDCAYAAADEfbZbAAAACXBIWXMAABJ0AAASdAHeZh94...RK5CYII='
                    ],
                    'generateUniqueName' => true,
                    'rights' => [
                        [
                            'TASK_ID' => 79,
                            'ACCESS_CODE' => 'U1271'
                        ]
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
        echo 'Error uploading file: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "disk.storage.uploadfile",
        {
            id: 1357,
            data: {
                NAME: "picture.png"
            },
            fileContent: [
            'picture.png',
            'iVBORw0KGgoAAAANSUhEUgAAAD4AAABDCAYAAADEfbZbAAAACXBIWXMAABJ0AAASdAHeZh94...RK5CYII=',
            ],
            generateUniqueName: true,
            rights: [
                {
                    TASK_ID: 79,
                    ACCESS_CODE: 'U1271'
                }
            ]
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
        'disk.storage.uploadfile',
        [
            'id' => 1357,
            'data' => [
                'NAME' => 'picture.png'
            ],
            'fileContent' => [
                'picture.png',
                'iVBORw0KGgoAAAANSUhEUgAAAD4AAABDCAYAAADEfbZbAAAACXBIWXMAABJ0AAASdAHeZh94...RK5CYII='
            ],
            'generateUniqueName' => true,
            'rights' => [
                [
                    'TASK_ID' => 79,
                    'ACCESS_CODE' => 'U1271'
                ]
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
        "ID": 9035,
        "NAME": "picture.png",
        "CODE": null,
        "STORAGE_ID": "1357",
        "TYPE": "file",
        "PARENT_ID": "8875",
        "DELETED_TYPE": 0,
        "GLOBAL_CONTENT_VERSION": 1,
        "FILE_ID": 32895,
        "SIZE": "1679",
        "CREATE_TIME": "2026-02-02T16:01:59+02:00",
        "UPDATE_TIME": "2026-02-02T16:01:59+02:00",
        "DELETE_TIME": null,
        "CREATED_BY": "1269",
        "UPDATED_BY": "1269",
        "DELETED_BY": null,
        "DOWNLOAD_URL": "https://test.bitrix24.com/rest/download.json?auth=b8d880690000071b006e2cf2000004f50000078dbaf74c54ad1b4e4205aba7ab57a395&token=disk%7CaWQ9OTAzNSZfPWU5eXpWQXpsVmJrdFE0OTJ3azBKQzNFVFVMek5UMTRU%7CImRvd25sb2FkfGRpc2t8YVdROU9UQXpOU1pmUFdVNWVYcFdRWHBzVm1KcmRGRTBPVEozYXpCS1F6TkZWRlZNZWs1VU1UUlV8YjhkODgwNjkwMDAwMDcxYjAwNmUyY2YyMDAwMDA0ZjUwMDAwMDc4ZGJhZjc0YzU0YWQxYjRlNDIwNWFiYTdhYjU3YTM5NSI%3D.DJafMz5LAuRzlGbCxNLoGiCleoFwz1qGyj4iPf7n110%3D",
        "DETAIL_URL": "https://test.bitrix24.com/company/personal/user/1269/disk/file/picture.png"
    },
    "time": {
        "start": 1770051719,
        "finish": 1770051719.707384,
        "duration": 0.7073841094970703,
        "processing": 0,
        "date_start": "2026-02-02T16:01:59+02:00",
        "date_finish": "2026-02-02T16:01:59+02:00",
        "operating_reset_at": 1770052319,
        "operating": 0.34273815155029297
    }
}
```
   
### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | An array with file fields ||
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
[`datetime`](../../data-types.md) | Date and time of the last file update ||
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
    "error":"ERROR_NOT_FOUND",
    "error_description":"Could not find entity with id `X`"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ERROR_ARGUMENT` | Invalid value of parameter {Parameter #0} | Required parameter not specified ||
|| `ERROR_NOT_FOUND` | Could not find entity with id `X` | Storage with the specified `id` not found ||
|| `DISK_BASE_SERVICE_22001` | Error: required parameter NAME (DISK_BASE_SERVICE_22001) | Required parameter `NAME` not specified in the `data` array ||
|| `ERROR_COULD_NOT_SAVE_FILE` | Could not save file | Failed to save the file. Check available space on the Drive and the correctness of the data encoding ||
|| — | Invalid rights format | Invalid format of the `rights` parameter ||
|| `ACCESS_DENIED` | Access denied | Insufficient permissions to add the file ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./disk-storage-add-folder.md)
- [{#T}](./disk-storage-get-children.md)
- [{#T}](./disk-storage-get-fields.md)
- [{#T}](./disk-storage-get-for-app.md)
- [{#T}](./disk-storage-get-list.md)
- [{#T}](./disk-storage-get-types.md)
- [{#T}](./disk-storage-get.md)
- [{#T}](./disk-storage-rename.md)