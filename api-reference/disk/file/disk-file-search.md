# Search Files and Folders disk.file.search

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `disk.file.search` finds files and folders on Drive by a text query.

The search works through an index that covers the names of files and folders, and for documents it also covers the text inside the file. The method does not find objects in the trash.

The result includes only the objects the current user has read access to. The method does not return objects from storages without internal access permissions — for example, from the storages of other modules. Chat folders are excluded from the results.

The method rejects a query shorter than three characters, so it is not suitable for suggestions based on the first few characters entered. To walk through a known structure, use the [disk.storage.getChildren](../storage/disk-storage-get-children.md) and [disk.folder.getChildren](../folder/disk-folder-get-children.md) methods, and if the file identifier is already known — [disk.file.get](./disk-file-get.md).

## Method Parameters

{% include [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **QUERY***
[`string`](../../data-types.md) | Text of the search query.

The length is from 3 to 255 characters. Repeated spaces collapse into one, leading and trailing spaces are trimmed, and the length is validated after that ||
|| **TYPE**
[`enum`](../../data-types.md) | Type of objects in the result:

- `file` — files only
- `folder` — folders only
- `all` — files and folders

The default value is `file` ||
|| **FILTER**
[`object`](../../data-types.md) | Search scope [(detailed description)](#filter).

Without this parameter, the method searches across all storages available to the user ||
|| **start**
[`integer`](../../data-types.md) | Offset for pagination.

The method returns no more than 50 objects per request. The value of the parameter is the number of skipped objects, not the page number: with `start=10`, the results begin with the eleventh object.

The default value is 0. The maximum value is 1000, and the method reduces a larger value to 1000.

The name of the parameter is written in lowercase, unlike the other parameters of the method ||
|#

### FILTER Parameter {#filter}

#|
|| **Name**
`type` | **Description** ||
|| **STORAGE_ID**
[`integer`](../../data-types.md) | Optional key. Identifier of the storage to search in.

You can retrieve the identifier using the [disk.storage.getList](../storage/disk-storage-get-list.md) method ||
|| **FOLDER_ID**
[`integer`](../../data-types.md) | Optional key. Identifier of the folder to search in. The search also covers nested folders, and the folder itself is not included in the result.

You can retrieve the identifier using the [disk.folder.getChildren](../folder/disk-folder-get-children.md) method ||
|#

Both keys can be passed together — in that case the method verifies that the folder belongs to the specified storage and returns the `NOT_FOUND` error if it does not.

The method does not accept other keys in `FILTER` and returns the `INVALID_FILTER` error.

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"QUERY":"test","TYPE":"all","FILTER":{"STORAGE_ID":1}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/disk.file.search
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"QUERY":"test","TYPE":"all","FILTER":{"STORAGE_ID":1},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/disk.file.search
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    // Fields marked optional come only for files or only for folders
    type DiskFileSearchItem = {
      ID: string
      NAME: string
      CODE: string | null
      STORAGE_ID: string
      TYPE: 'file' | 'folder'
      REAL_OBJECT_ID?: string
      PARENT_ID: string
      DELETED_TYPE: string
      GLOBAL_CONTENT_VERSION?: string
      FILE_ID?: string
      SIZE?: string
      CREATE_TIME: ISODate
      UPDATE_TIME: ISODate
      DELETE_TIME: ISODate | null
      CREATED_BY: string
      UPDATED_BY: string
      DELETED_BY: string
      DOWNLOAD_URL?: string
      DETAIL_URL: string | null
    }

    try {
      const response = await $b24.actions.v2.call.make<DiskFileSearchItem[]>({
        method: 'disk.file.search',
        params: {
          QUERY: 'test',
          TYPE: 'all',
          FILTER: {
            STORAGE_ID: 1,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        result.forEach((item) => console.info(item.ID, item.TYPE, item.NAME))
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
      async function searchDiskFiles() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'disk.file.search',
            params: {
              QUERY: 'test',
              TYPE: 'all',
              FILTER: {
                STORAGE_ID: 1
              }
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          result.forEach(function (item) {
            console.info(item.ID, item.TYPE, item.NAME)
          })
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', searchDiskFiles)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'disk.file.search',
                [
                    'QUERY' => 'test',
                    'TYPE' => 'all',
                    'FILTER' => [
                        'STORAGE_ID' => 1
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
        echo 'Error searching files: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "disk.file.search",
        {
            QUERY: "test",
            TYPE: "all",
            FILTER: {
                STORAGE_ID: 1
            }
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
        'disk.file.search',
        [
            'QUERY' => 'test',
            'TYPE' => 'all',
            'FILTER' => [
                'STORAGE_ID' => 1
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "disk.file.search", b24.Params{
    	"QUERY": "test",
    	"TYPE":  "all",
    	"FILTER": b24.Params{
    		"STORAGE_ID": 1,
    	},
    })
    if err != nil {
    	return fmt.Errorf("disk.file.search: %w", err)
    }

    var items []struct {
    	ID           b24.ID `json:"ID"`
    	Name         string `json:"NAME"`
    	StorageID    b24.ID `json:"STORAGE_ID"`
    	Type         string `json:"TYPE"`
    	RealObjectID b24.ID `json:"REAL_OBJECT_ID"`
    	ParentID     b24.ID `json:"PARENT_ID"`
    }
    if err := json.Unmarshal(res.Result, &items); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, it := range items {
    	fmt.Println(it.ID, it.Name)
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": [
        {
            "ID": "1739",
            "NAME": "New Folder for Process Testing",
            "CODE": null,
            "STORAGE_ID": "1",
            "TYPE": "folder",
            "REAL_OBJECT_ID": "1739",
            "PARENT_ID": "649",
            "DELETED_TYPE": "0",
            "CREATE_TIME": "2020-10-26T16:25:33+02:00",
            "UPDATE_TIME": "2024-11-26T09:23:03+02:00",
            "DELETE_TIME": null,
            "CREATED_BY": "0",
            "UPDATED_BY": "1",
            "DELETED_BY": "0",
            "DETAIL_URL": "https://test.bitrix24.com/company/personal/user/1/disk/path/Created files/New Folder for Process Testing"
        },
        {
            "ID": "1277",
            "NAME": "Klaus Weber - Software Testing.pdf",
            "CODE": null,
            "STORAGE_ID": "1",
            "TYPE": "file",
            "PARENT_ID": "1275",
            "DELETED_TYPE": "0",
            "GLOBAL_CONTENT_VERSION": "1",
            "FILE_ID": "1983",
            "SIZE": "5517483",
            "CREATE_TIME": "2020-08-07T15:43:48+02:00",
            "UPDATE_TIME": "2020-08-07T15:43:48+02:00",
            "DELETE_TIME": null,
            "CREATED_BY": "1",
            "UPDATED_BY": "1",
            "DELETED_BY": "0",
            "DOWNLOAD_URL": "https://test.bitrix24.com/rest/download.json?auth=**put_access_token_here**&token=disk%7CaWQ9MTI3NyZfPXVqVGJUMmxoclBOb0JmQjVLWmxyWnRISWFTQ2M5V2hT",
            "DETAIL_URL": "https://test.bitrix24.com/company/personal/user/1/disk/file/Uploaded files/Imported files/Klaus Weber - Software Testing.pdf"
        }
    ],
    "time": {
        "start": 1785494344,
        "finish": 1785494344.440217,
        "duration": 0.4402170181274414,
        "processing": 0,
        "date_start": "2026-07-31T13:39:04+02:00",
        "date_finish": "2026-07-31T13:39:04+02:00",
        "operating_reset_at": 1785494944,
        "operating": 0.13181495666503906
    }
}
```

If nothing is found, the method returns an empty array:

```json
{
    "result": [],
    "time": {
        "start": 1785496443,
        "finish": 1785496443.510742,
        "duration": 0.5107419490814209,
        "processing": 0,
        "date_start": "2026-07-31T14:14:03+02:00",
        "date_finish": "2026-07-31T14:14:03+02:00",
        "operating_reset_at": 1785497043,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | Array of found objects [(detailed description)](#result) ||
|| **next**
[`integer`](../../data-types.md) | Offset for the next request. It comes only when there is a next page [(example)](#pagination) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Object in the result Array {#result}

The set of fields depends on the type of the object. The `GLOBAL_CONTENT_VERSION`, `FILE_ID`, `SIZE`, and `DOWNLOAD_URL` fields come only for files, and the `REAL_OBJECT_ID` field comes only for folders.

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../../data-types.md) | Identifier of the object ||
|| **NAME**
[`string`](../../data-types.md) | Name of the file or folder ||
|| **CODE**
[`string`](../../data-types.md) | Symbolic code of the object. It comes as `null` if the code is not set ||
|| **STORAGE_ID**
[`integer`](../../data-types.md) | Identifier of the storage the object is located in ||
|| **TYPE**
[`enum`](../../data-types.md) | Type of the object: `file` or `folder` ||
|| **REAL_OBJECT_ID**
[`integer`](../../data-types.md) | Identifier of the folder the object refers to. For a regular folder, it matches `ID`. It comes only for folders ||
|| **PARENT_ID**
[`integer`](../../data-types.md) | Identifier of the parent folder ||
|| **DELETED_TYPE**
[`enum`](../../data-types.md) | Deletion status of the object. The method returns only objects with the value `0` — not deleted ||
|| **GLOBAL_CONTENT_VERSION**
[`integer`](../../data-types.md) | Incremental counter of the file version. It comes only for files ||
|| **FILE_ID**
[`integer`](../../data-types.md) | Internal value of the file identifier. It comes only for files ||
|| **SIZE**
[`integer`](../../data-types.md) | Size of the file in bytes. It comes only for files ||
|| **CREATE_TIME**
[`datetime`](../../data-types.md) | Date and time the object was created ||
|| **UPDATE_TIME**
[`datetime`](../../data-types.md) | Date and time the object was last updated ||
|| **DELETE_TIME**
[`datetime`](../../data-types.md) | Date and time the object was moved to the trash. The method does not return objects from the trash, so the field is always `null` ||
|| **CREATED_BY**
[`integer`](../../data-types.md) | Identifier of the user who created the object ||
|| **UPDATED_BY**
[`integer`](../../data-types.md) | Identifier of the user who made the last change ||
|| **DELETED_BY**
[`integer`](../../data-types.md) | Identifier of the user who deleted the object ||
|| **DOWNLOAD_URL**
[`string`](../../data-types.md) | Link to download the file. It comes only for files ||
|| **DETAIL_URL**
[`string`](../../data-types.md) | Link to open the object in the interface. For objects from storages that do not belong to Drive, it comes as `null` ||
|#

The objects are sorted by the date of the last change, from newest to oldest.

### Pagination {#pagination}

The method returns no more than 50 objects per request. If more are found, the `next` field comes next to `result` — the offset the next page starts from. The method does not return the total number of found objects.

The response of the first page, with `result` in the example shortened to 2 objects out of 50:

```json
{
    "result": [
        {
            "ID": "9739",
            "NAME": "report-51.txt",
            "CODE": null,
            "STORAGE_ID": "1",
            "TYPE": "file",
            "PARENT_ID": "9637",
            "DELETED_TYPE": "0",
            "GLOBAL_CONTENT_VERSION": "1",
            "FILE_ID": "36819",
            "SIZE": "6",
            "CREATE_TIME": "2026-07-31T14:15:49+02:00",
            "UPDATE_TIME": "2026-07-31T14:15:49+02:00",
            "DELETE_TIME": null,
            "CREATED_BY": "1",
            "UPDATED_BY": "1",
            "DELETED_BY": "0",
            "DOWNLOAD_URL": "https://test.bitrix24.com/rest/download.json?auth=**put_access_token_here**&token=disk%7CaWQ9OTczOSZfPTFEU1hGMGtkY3E2Q3FZUTIyM2tiV3R6Tk5jZHgxMzR2",
            "DETAIL_URL": "https://test.bitrix24.com/company/personal/user/1/disk/file/Reports/report-51.txt"
        },
        {
            "ID": "9737",
            "NAME": "report-50.txt",
            "CODE": null,
            "STORAGE_ID": "1",
            "TYPE": "file",
            "PARENT_ID": "9637",
            "DELETED_TYPE": "0",
            "GLOBAL_CONTENT_VERSION": "1",
            "FILE_ID": "36817",
            "SIZE": "6",
            "CREATE_TIME": "2026-07-31T14:15:48+02:00",
            "UPDATE_TIME": "2026-07-31T14:15:48+02:00",
            "DELETE_TIME": null,
            "CREATED_BY": "1",
            "UPDATED_BY": "1",
            "DELETED_BY": "0",
            "DOWNLOAD_URL": "https://test.bitrix24.com/rest/download.json?auth=**put_access_token_here**&token=disk%7CaWQ9OTczNyZfPTBmQlVlRDFCMTA0ajNhc3ZwbFdVRnhXNUg1MFpha3JO",
            "DETAIL_URL": "https://test.bitrix24.com/company/personal/user/1/disk/file/Reports/report-50.txt"
        }
    ],
    "next": 50,
    "time": {
        "start": 1785496566,
        "finish": 1785496566.197965,
        "duration": 0.19796490669250488,
        "processing": 0,
        "date_start": "2026-07-31T14:16:06+02:00",
        "date_finish": "2026-07-31T14:16:06+02:00",
        "operating_reset_at": 1785497166,
        "operating": 0
    }
}
```

Pass the value of `next` in the `start` parameter:

```bash
curl -X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-d '{"QUERY":"report","start":50}' \
https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/disk.file.search
```

On the last page, the response has no `next` field:

```json
{
    "result": [
        {
            "ID": "9639",
            "NAME": "report-01.txt",
            "CODE": null,
            "STORAGE_ID": "1",
            "TYPE": "file",
            "PARENT_ID": "9637",
            "DELETED_TYPE": "0",
            "GLOBAL_CONTENT_VERSION": "1",
            "FILE_ID": "36719",
            "SIZE": "6",
            "CREATE_TIME": "2026-07-31T14:14:56+02:00",
            "UPDATE_TIME": "2026-07-31T14:14:56+02:00",
            "DELETE_TIME": null,
            "CREATED_BY": "1",
            "UPDATED_BY": "1",
            "DELETED_BY": "0",
            "DOWNLOAD_URL": "https://test.bitrix24.com/rest/download.json?auth=**put_access_token_here**&token=disk%7CaWQ9OTYzOSZfPXFPZWc1bnBGZFFPcXd0anlzd3BHN2VEQ3c4UXlGdk5l",
            "DETAIL_URL": "https://test.bitrix24.com/company/personal/user/1/disk/file/Reports/report-01.txt"
        }
    ],
    "time": {
        "start": 1785496584,
        "finish": 1785496584.545265,
        "duration": 0.5452649593353271,
        "processing": 0,
        "date_start": "2026-07-31T14:16:24+02:00",
        "date_finish": "2026-07-31T14:16:24+02:00",
        "operating_reset_at": 1785497184,
        "operating": 0
    }
}
```

The navigation depth is limited: the method reduces an offset greater than 1000 to 1000. The results do not move past the object number 1050, so there is no point in increasing `start` indefinitely. If there are too many results, narrow the search scope with the `FILTER` parameter.

## Error Handling

HTTP status: **400**

```json
{
    "error": "INVALID_QUERY",
    "error_description": "Search query is invalid. (INVALID_QUERY)."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ERROR_ARGUMENT` | Invalid value of parameter { Parameter #0 [ `<required>` $QUERY ] }. | The required `QUERY` parameter is not passed ||
|| `INVALID_QUERY` | Search query is invalid. (INVALID_QUERY). | The query is shorter than 3 or longer than 255 characters, or it is not passed as a string ||
|| `INVALID_TYPE` | Search result type is invalid. (INVALID_TYPE). | The `TYPE` value differs from `file`, `folder`, and `all` ||
|| `INVALID_FILTER` | Search filter contains an unknown field. (INVALID_FILTER). | A key other than `STORAGE_ID` and `FOLDER_ID` is passed in `FILTER` ||
|| `NOT_FOUND` | Search scope was not found. (NOT_FOUND). | The storage or folder from `FILTER` does not exist, is not available to the user for reading, or the folder does not belong to the specified storage. The same error comes if `FILTER` is not passed as an object ||
|| `UNSUPPORTED_STORAGE` | Search is not supported for this storage. (UNSUPPORTED_STORAGE). | The specified storage does not use the internal access permissions of Drive ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./disk-file-copy-to.md)
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
