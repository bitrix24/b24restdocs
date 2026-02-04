# Upload a New File to the Specified Folder disk.folder.uploadfile

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: a user with "Add" access permission for the required folder

The method `disk.folder.uploadfile` uploads a new file to the specified folder.

## Method Parameters

{% include [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Identifier of the folder where the file needs to be uploaded.

The identifier can be obtained using the method [disk.storage.getchildren](../storage/disk-storage-get-children.md) if the folder is in the root of the storage, and using the method [disk.folder.getchildren](./disk-folder-get-children.md) if the folder is in another folder
||
|| **data***
[`array`](../../data-types.md) | An array with the field `NAME`, where `NAME` is the name of the file.

Optional if the file is uploaded not directly, but via URL. An example of uploading a file via URL is provided [below](#uploadurl)  ||
|| **fileContent**
[`array`](../../data-types.md) | An array containing the file name and a string with [Base64](../../files/how-to-upload-files.md).

If the parameter is not provided, the method does not upload the file but returns the URL for uploading `UploadUrl` and the form field name `field` ||
|| **rights**
[`array`](../../data-types.md) | An array of access permissions for the uploaded file in the format `{"TASK_ID": 42, "ACCESS_CODE": "U35"}`, where
- `TASK_ID` — access level identifier
- `ACCESS_CODE` — access code consisting of the user's or department's letter code and identifier

User categories:
- `U` — user
- `*` — all users
- `D` — all department employees
- `DR` — all department employees with subdivisions

The list of available `TASK_ID` identifiers for setting permissions can be obtained using the method [disk.rights.getTasks](../rights/disk-rights-get-tasks.md) ||
|| **generateUniqueName**
[`boolean`](../../data-types.md) | Generate a unique file name if a file with that name already exists. For example, file (1).docx.

Possible values:
- `true` — generate
- `false` — do not generate

Default is `false` ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

### Direct File Upload

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":8930,"data":{"NAME":"test.png"},"fileContent":["test.png","iVBORw0KGgoAAAANSUhEUgAAAD4AAABDCAYAAADEfbZbAAAACXBIWXMAABJ0AAASdAHeZh94...rk5CYII="],"generateUniqueName":true,"rights":[{"TASK_ID":75,"ACCESS_CODE":"U1271"}]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/disk.folder.uploadfile
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":8930,"data":{"NAME":"test.png"},"fileContent":["test.png","iVBORw0KGgoAAAANSUhEUgAAAD4AAABDCAYAAADEfbZbAAAACXBIWXMAABJ0AAASdAHeZh94...rk5CYII="],"generateUniqueName":true,"rights":[{"TASK_ID":75,"ACCESS_CODE":"U1271"}],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/disk.folder.uploadfile
    ```

- JS

    ```js
    try {
        const response = await $b24.callMethod(
            'disk.folder.uploadfile',
            {
                id: 8930,
                data: {
                    NAME: 'test.png',
                },
                fileContent: [
                    'test.png',
                    'iVBORw0KGgoAAAANSUhEUgAAAD4AAABDCAYAAADEfbZbAAAACXBIWXMAABJ0AAASdAHeZh94...rk5CYII=',
                ],
                generateUniqueName: true,
                rights: [
                    {
                        TASK_ID: 75,
                        ACCESS_CODE: 'U1271'
                    }
                ]
            }
        );

        const result = response.getData().result;
        console.log(result);
        processResult(result);
    } catch (error) {
        console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'disk.folder.uploadfile',
                [
                    'id' => 8930,
                    'data' => [
                        'NAME' => 'test.png'
                    ],
                    'fileContent' => [
                        'test.png',
                        'iVBORw0KGgoAAAANSUhEUgAAAD4AAABDCAYAAADEfbZbAAAACXBIWXMAABJ0AAASdAHeZh94...rk5CYII='
                    ],
                    'generateUniqueName' => true,
                    'rights' => [
                        [
                            'TASK_ID' => 75,
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
        "disk.folder.uploadfile",
        {
            id: 8930,
            data: {
                NAME: "test.png"
            },
            fileContent: [
            'test.png',
            'iVBORw0KGgoAAAANSUhEUgAAAD4AAABDCAYAAADEfbZbAAAACXBIWXMAABJ0AAASdAHeZh94...rk5CYII=',
            ],
            generateUniqueName: true,
            rights: [
                {
                    TASK_ID: 75,
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
        'disk.folder.uploadfile',
        [
            'id' => 8930,
            'data' => [
                'NAME' => 'test.png'
            ],
            'fileContent' => [
                'test.png',
                'iVBORw0KGgoAAAANSUhEUgAAAD4AAABDCAYAAADEfbZbAAAACXBIWXMAABJ0AAASdAHeZh94...rk5CYII='
            ],
            'generateUniqueName' => true,
            'rights' => [
                [
                    'TASK_ID' => 75,
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

### Uploading a File via URL {#uploadurl}

1. Call the method and pass only the `ID` of the folder.

    {% list tabs %}

    - cURL (Webhook)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"id":8930}' \
        https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/disk.folder.uploadfile
        ```

    - cURL (OAuth)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"id":8930,"auth":"**put_access_token_here**"}' \
        https://**put_your_bitrix24_address**/rest/disk.folder.uploadfile
        ```

    - JS

        ```js
        try
        {
            const response = await $b24.callMethod(
                'disk.folder.uploadfile',
                {
                    id: 8930,
                }
            );
            
            const result = response.getData().result;
            console.log(result);
            
            processResult(result);
        }
        catch( error )
        {
            console.error('Error:', error);
        }
        ```

    - PHP

        ```php
        try {
            $response = $b24Service
                ->core
                ->call(
                    'disk.folder.uploadfile',
                    [
                        'id' => 8930
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
            "disk.folder.uploadfile",
            {
                id: 8930
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
            'disk.folder.uploadfile',
            [
                'id' => 8930
            ]
        );

        echo '<PRE>';
        print_r($result);
        echo '</PRE>';
        ```

    {% endlist %}

2. In response, you will receive a URL for uploading `UploadUrl` and the form field name `field`.

    ```json
    "result": {
            "field": "file",
            "uploadUrl": "https://test.bitrix24.com/rest/upload.json?auth=929b78690000071b006e2cf2000004f5000007dde54ef79d3b6e5c447d8f8a714563bd&token=disk%7CaWQ9ODkzMCZnZW5lcmF0ZVVuaXF1ZU5hbWU9MCZfPU4zS0pqUFJiVDFSengzUmpUaTVPcGM4R1VQTlFkTWU0%7CInVwbG9hZHxkaXNrfGFXUTlPRGt6TUNablpXNWxjbUYwWlZWdWFYRjFaVTVoYldVOU1DWmZQVTR6UzBwcVVGSmlWREZTZW5nelVtcFVhVFZQY0dNNFIxVlFUbEZrVFdVMHw5MjliNzg2OTAwMDAwNzFiMDA2ZTJjZjIwMDAwMDRmNTAwMDAwN2RkZTU0ZWY3OWQzYjZlNWM0NDdkOGY4YTcxNDU2M2JkIg%3D%3D.OHwSxVni%2FKX9Pw%2FyMzpfR974ImX5bC0sigTqA0UTCp8%3D"
            },
        "time": {
            "start": 1769511710,
            "finish": 1769511710.411701,
            "duration": 0.411700963973999,
            "processing": 0,
            "date_start": "2026-01-27T14:01:50+02:00",
            "date_finish": "2026-01-27T14:01:50+02:00",
            "operating_reset_at": 1769512310,
            "operating": 0
        }
    ```

3. Send the file to the received address `UploadUrl` using a POST request with the type `multipart/form-data`. The field name for the file inside this request must match the value of the `field` parameter from the response.

    ```
    http --form POST "https://test.bitrix24.com/rest/upload.json?auth=929b78690000071b006e2cf2000004f5000007dde54ef79d3b6e5c447d8f8a714563bd&token=disk%7CaWQ9ODkzMCZnZW5lcmF0ZVVuaXF1ZU5hbWU9MCZfPU4zS0pqUFJiVDFSengzUmpUaTVPcGM4R1VQTlFkTWU0%7CInVwbG9hZHxkaXNrfGFXUTlPRGt6TUNablpXNWxjbUYwWlZWdWFYRjFaVTVoYldVOU1DWmZQVTR6UzBwcVVGSmlWREZTZW5nelVtcFVhVFZQY0dNNFIxVlFUbEZrVFdVMHw5MjliNzg2OTAwMDAwNzFiMDA2ZTJjZjIwMDAwMDRmNTAwMDAwN2RkZTU0ZWY3OWQzYjZlNWM0NDdkOGY4YTcxNDU2M2JkIg%3D%3D.OHwSxVni%2FKX9Pw%2FyMzpfR974ImX5bC0sigTqA0UTCp8%3D" file@/path/to/file.png
    ```

4. In case of success, the server will return an array with data about the uploaded file.

### Uploading a File via URL in PHP

{% list tabs %}

- PHP CRest

    ```php
    <?php
    require_once (__DIR__.'/crest.php');

    $path = __DIR__ . '/pic.jpg';
    $folderId = 1;

    $result = [];
    if (file_exists($path))
    {
        $file = CRest::call(
            'disk.folder.uploadfile',
            [
                'id' => $folderId,
            ]
        );
        if (!empty($file['result']['uploadUrl']))
        {
            $info = pathinfo($path);
            if ($info['basename'])
            {
                $delimiter = '-------------' . uniqid('', true);
                $name = $info['basename'];
                $mime = mime_content_type($path);
                $content = file_get_contents($path);

                $body = '--' . $delimiter. "\r\n";
                $body .= 'Content-Disposition: form-data; name="file"';
                $body .= '; filename="' . $name . '"' . "\r\n";
                $body .= 'Content-Type: ' . $mime . "\r\n\r\n";
                $body .= $content . "\r\n";
                $body .= "--" . $delimiter . "--\r\n";

                $ch = curl_init();
                curl_setopt($ch, CURLOPT_URL, $file['result']['uploadUrl']);
                curl_setopt($ch, CURLOPT_POST, 1);
                curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
                curl_setopt($ch, CURLOPT_POSTFIELDS, $body);
                curl_setopt(
                    $ch,
                    CURLOPT_HTTPHEADER,
                    [
                        'Content-Type: multipart/form-data; boundary=' . $delimiter,
                        'Content-Length: ' . strlen($body),
                    ]
                );
                $out = curl_exec($ch);
                try
                {
                    $result = json_decode($out, true, 512, JSON_THROW_ON_ERROR);
                }
                catch (JsonException $e)
                {
                    $result = [
                        'error' => $e->getMessage(),
                    ];
                }
            }
        }
    }

    echo '<pre>';
        print_r($result);
    echo '</pre>';
    ?>
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "ID": 9011,
        "NAME": "test.png",
        "CODE": null,
        "STORAGE_ID": "1357",
        "TYPE": "file",
        "PARENT_ID": "8930",
        "DELETED_TYPE": 0,
        "GLOBAL_CONTENT_VERSION": 1,
        "FILE_ID": 32877,
        "SIZE": "1679",
        "CREATE_TIME": "2026-01-27T13:06:55+02:00",
        "UPDATE_TIME": "2026-01-27T13:06:55+02:00",
        "DELETE_TIME": null,
        "CREATED_BY": "1269",
        "UPDATED_BY": "1269",
        "DELETED_BY": null,
        "DOWNLOAD_URL": "https://test.bitrix24.com/rest/download.json?auth=929b78690000071b006e2cf2000004f5000007dde54ef79d3b6e5c447d8f8a714563bd&token=disk%7CaWQ9OTAxMSZfPUFKQTNyWWVLZkxVdEw0VDRjY1QyOGpyN1NqYXZneFRI%7CImRvd25sb2FkfGRpc2t8YVdROU9UQXhNU1pmUFVGS1FUTnlXV1ZMWmt4VmRFdzBWRFJqWTFReU9HcHlOMU5xWVhabmVGUkl8OTI5Yjc4NjkwMDAwMDcxYjAwNmUyY2YyMDAwMDA0ZjUwMDAwMDdkZGU1NGVmNzlkM2I2ZTVjNDQ3ZDhmOGE3MTQ1NjNiZCI%3D.XX%2BUFpxsl2eoLuCwolEaMKsrAJ5IIpIPmzg1j6QOuE0%3D",
        "DETAIL_URL": "https://test.bitrix24.com/company/personal/user/1269/disk/file/Folder/Folder in folder/test.png"
    },
    "time": {
        "start": 1769508415,
        "finish": 1769508416.040339,
        "duration": 1.0403389930725098,
        "processing": 1,
        "date_start": "2026-01-27T13:06:55+02:00",
        "date_finish": "2026-01-27T13:06:56+02:00",
        "operating_reset_at": 1769509015,
        "operating": 0.7169051170349121
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
[`string`](../../data-types.md) | Incremental version counter of the file ||
|| **FILE_ID**
[`integer`](../../data-types.md) | Internal value of the file identifier ||
|| **SIZE**
[`integer`](../../data-types.md) | Size of the file in bytes ||
|| **CREATE_TIME**
[`datetime`](../../data-types.md) | Date and time of file creation ||
|| **UPDATE_TIME**
[`datetime`](../../data-types.md) | Date and time of the last file update ||
|| **DELETE_TIME**
[`datetime`](../../data-types.md) | Date and time of moving the file to the trash ||
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
|| `ERROR_ARGUMENT` | Invalid value of parameter {Parameter #0} | The required parameter `id` is not specified ||
|| `ERROR_NOT_FOUND` | Could not find entity with id `X` | The folder with the specified `id` was not found ||
|| `DISK_BASE_SERVICE_22001` | Error: required parameter NAME (DISK_BASE_SERVICE_22001) | The required parameter `NAME` in the `data` array is not specified ||
|| `ERROR_COULD_NOT_SAVE_FILE` | Could not save file | Failed to save the file. Check free space on the Disk and the correctness of data encoding ||
|| `ACCESS_DENIED` | Access denied | Insufficient rights to add the file ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./disk-folder-add-subfolder.md)
- [{#T}](./disk-folder-copy-to.md)
- [{#T}](./disk-folder-delete-tree.md)
- [{#T}](./disk-folder-get-children.md)
- [{#T}](./disk-folder-get-external-link.md)
- [{#T}](./disk-folder-get-fields.md)
- [{#T}](./disk-folder-get.md)
- [{#T}](./disk-folder-move-to.md)
- [{#T}](./disk-folder-rename.md)
- [{#T}](./disk-folder-restore.md)
- [{#T}](./disk-folder-share-to-user.md)
- [{#T}](../../../tutorials/tasks/how-to-upload-file-to-task.md)
- [{#T}](../../../tutorials/tasks/how-to-create-task-with-file.md) 
- [{#T}](../../../tutorials/tasks/how-to-create-comment-with-file.md)