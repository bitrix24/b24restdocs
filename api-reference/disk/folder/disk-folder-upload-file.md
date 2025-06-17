# Upload a New File to the Specified Folder disk.folder.uploadfile

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- no response in case of error

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly

{% endnote %}

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `disk.folder.uploadfile` uploads a new file to the specified folder.

## Parameters

#|
||  **Parameter** / **Type**| **Description** ||
|| **id**
[`unknown`](../../data-types.md) | Folder identifier. In the current API, it is not possible to upload a file by the folder path. It is necessary to compute the `ID` of the folder. ||
|| **fileContent**
[`unknown`](../../data-types.md) | Upload the file in [Base64](../../files/how-to-upload-files.md) format. ||
|| **data**
[`unknown`](../../data-types.md) | An array describing the file. The required field `NAME` — the name of the new file. It is possible to send the file as a string encoded in base64. ||
|| **generateUniqueName**
[`unknown`](../../data-types.md) | Optional, defaults to `false`. If set to `true`, the uploaded file will have a unique name by adding a suffix (1), (2), etc. Example: avatar (1).jpg, avatar (2).jpg. ||
|| **rights**
[`unknown`](../../data-types.md) | Optional, defaults to an empty array. An array of access permissions for the uploaded file. ||
|#

## Examples

{% note info %}

Please note that the list of available `TASK_ID` identifiers for setting permissions can be obtained using the method [disk.rights.getTasks](../rights/disk-rights-get-tasks.md)

{% endnote %}

### Example of File Upload

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        "disk.folder.uploadfile",
        {
            id: 4,
            data: {
                NAME: "avatar.jpg"
            },
            fileContent: document.getElementById('test_file_input'),
            generateUniqueName: true,
            rights: [
                {
                    TASK_ID: 42,
                    ACCESS_CODE: 'U35' // access for user with ID=35
                },
                {
                    TASK_ID: 38,
                    ACCESS_CODE: 'U2' // access for user with ID=2
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

{% endlist %}

{% include [Footnote on examples](../../../_includes/examples.md) %}

### Example of Direct File Upload to Disk

1. Call `/rest/disk.folder.uploadFile` and pass only the `ID` of the folder to the method.
    ```
    disk.folder.uploadFile?auth=n2423m863oil59f99c9g0bm4918l5erz&id=289
    ```
2. In response, receive the `UploadUrl` parameter and the `field` parameter.
    ```json
    "result": {
    "field": "file",
    "uploadUrl": "http://b24.sigurd.bx/rest/upload.json?auth=n2423m863oil59f99c9g0bm4918l5erz&token=disk%7CaWQ9Mjg5Jl89QkYzazEzaXNnUjNHcVZQcDJZaGxGRmI4TGhXOG5EZXQ%3D%7CInVwbG9hZHxkaXNrfGFXUTlNamc1Smw4OVFrWXphekV6YVhOblVqTkhjVlpRY0RKWmFHeEdSbUk0VEdoWE9HNUVaWFE9fG4yNDIzbTg2M29pbDU5Zjk5YzlnMGJtNDkxOGw1ZXJ6Ig%3D%3D.Aga709nyY0%2BrFiv3laHjfg6XuOO5JT6ttjU%2F53ifphM%3D"
    }
    ```
3. Send a POST request to the received `UploadUrl` in `multipart/form-data`, where the file is passed in the field with the name received in the `field` parameter.
    ``` 
    http --form POST "http://b24.sigurd.bx/rest/upload.json?auth=n2423m863oil59f99c9g0bm4918l5erz&token=disk%7CaWQ9Mjg5Jl89QkYzazEzaXNnUjNHcVZQcDJZaGxGRmI4TGhXOG5EZXQ%3D%7CInVwbG9hZHxkaXNrfGFXUTlNamc1Smw4OVFrWXphekV6YVhOblVqTkhjVlpRY0RKWmFHeEdSbUk0VEdoWE9HNUVaWFE9fG4yNDIzbTg2M29pbDU5Zjk5YzlnMGJtNDkxOGw1ZXJ6Ig%3D%3D.Aga709nyY0%2BrFiv3laHjfg6XuOO5JT6ttjU%2F53ifphM%3D" file@~/somelongfile.log
    ```
4. In response, receive data about the uploaded file.
    ```json
    "result": {
    "CODE": null,
    "CREATED_BY": "1",
    "CREATE_TIME": "2016-03-30T14:30:41+02:00",
    "DELETED_BY": null,
    "DELETED_TYPE": 0,
    "DELETE_TIME": null,
    "DETAIL_URL": "http://b24.sigurd.bx/company/personal/user/1/disk/file/Testing REST/somelongfile.log",
    "DOWNLOAD_URL": "http://b24.sigurd.bx/rest/download.json?auth=n2423m863oil59f99c9g0bm4918l5erz&token=disk%7CaWQ9MjkwJl89ZTI4MG9TcDZCQno2MDAwVmV3cnRkbWxLM2hLN0JweEs%3D%7CImRvd25sb2FkfGRpc2t8YVdROU1qa3dKbDg5WlRJNE1HOVRjRFpDUW5vMk1EQXdWbVYzY25Sa2JXeExNMmhMTjBKd2VFcz18bjI0MjNtODYzb2lsNTlmOTljOWcwYm00OTE4bDVlcnoi.QlpUpx4mG9sxeyMyholPfdgkoXgc9kK9gtbOagqSo7s%3D",
    "FILE_ID": 209,
    "GLOBAL_CONTENT_VERSION": 1,
    "ID": 290,
    "NAME": "somelongfile.log",
    "PARENT_ID": "289",
    "SIZE": "496136787",
    "STORAGE_ID": "1",
    "TYPE": "file",
    "UPDATED_BY": "1",
    "UPDATE_TIME": "2016-03-30T14:30:43+02:00"
    }
    ```

### How to Upload a File via `UploadUrl` in PHP

{% list tabs %}

- PHP (crest)

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

## Response on Success

> 200 OK

On success, it returns a structure similar to [disk.file.get](../file/disk-file-get.md).

## Continue Learning

- [{#T}](../../../tutorials/tasks/how-to-create-comment-with-file.md)
- [{#T}](../../../tutorials/tasks/how-to-create-task-with-file.md)
- [{#T}](../../../tutorials/tasks/how-to-upload-file-to-task.md)