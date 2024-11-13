# How to Upload a File to a Task

> Scope: [`disk`, `tasks`](../../api-reference/scopes/permissions.md)
>
> Who can execute the method: users with access to the disk and tasks sections

In Bitrix24, there are two types of file fields:

* **File.** This field is not linked to the disk; files are uploaded directly via the [Base64 format string](../../api-reference/bx24-js-sdk/how-to-call-rest-methods/files.md).
* **File (disk).** This field is linked to the disk, storing the ID of the disk object. The Base64 format is not processed in this field, so the file must first be uploaded to the Bitrix24 disk.

To attach a file to a task, you need to sequentially execute two methods:

1. [disk.folder.uploadfile](../../api-reference/disk/folder/disk-folder-upload-file.md) — this method uploads a file to the disk.
2. [tasks.task.files.attach](../../api-reference/tasks/tasks-task-files-attach.md) — this method attaches the disk file to the task.

## 1. Uploading a File to the Bitrix24 Disk

To upload a file to the disk, we use the [disk.folder.uploadfile](../../api-reference/disk/folder/disk-folder-upload-file.md) method with the following parameters:

* `id` — specify the value `1739` — the identifier of the disk folder where the file will be uploaded.
* `data` — specify the file name `NAME`, which will be used to save the file on the Bitrix24 disk.
* `fileContent` — pass the file in the format ['file_name.extension', 'file as a Base64 encoded string'].

Uploading the file to the disk is a necessary step, as the `UF_TASK_WEBDAV_FILES` field in tasks only accepts disk file IDs.

{% include [Note on Examples](../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```javascript
    BX24.callMethod(
        "disk.folder.uploadfile",
        {
            id: 1739,
            data: {
                NAME: "ava555.jpg"
            },
            fileContent: [
                'avatar.jpg',
                '/9j/4AAQSkZJRgABAQEASABIAAD/2wBDAAQDAwQDAwQEAwQ///+dAYq6YFKoAv/AFnAa6ArKv8AAtFJVppxCEAulxQ2DWgfMR//2Q=='
            ]
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'disk.folder.uploadfile',
        [
            'id' => 1739,
            'data' => [
                'NAME' => 'ava555.jpg'
            ],
            'fileContent' => [
                'avatar.jpg',
                '/9j/4AAQSkZJRgABAQEASABIAAD/2wBDAAQDAwQDAwQEAwQ///+dAYq6YFKoAv/AFnAa6ArKv8AAtFJVppxCEAulxQ2DWgfMR//2Q=='
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

As a result of uploading the file to the disk, we received two different file ID values:

* `FILE_ID`: `28073` — the internal file ID value.
* `ID`: `6687` — the disk object ID; this value is used in methods for working with "file (disk)" type fields. 
If the request to change the "file (disk)" field passes the `FILE_ID` value, the file will either not attach to the task because there is no disk object with that ID, or the wrong file will be attached.

```json
{
    "result": {
        "ID": 6687,
        "NAME": "ava555.jpg",
        "CODE": null,
        "STORAGE_ID": "1",
        "TYPE": "file",
        "PARENT_ID": "1739",
        "DELETED_TYPE": 0,
        "GLOBAL_CONTENT_VERSION": 1,
        "FILE_ID": 28073,
        "SIZE": "405559",
        "CREATE_TIME": "2024-11-01T17:00:55+02:00",
        "UPDATE_TIME": "2024-11-01T17:00:55+02:00",
        "DELETE_TIME": null,
        "CREATED_BY": "1",
        "UPDATED_BY": "1",
        "DELETED_BY": null,
        "DOWNLOAD_URL": "https://your-domain.bitrix24.com/rest/download.json?sessid=9dd90ed5a58ccc41af81f5f0043739db&token=disk%7CaWQ9NjY4NyZfPTJ5ZXdvN2Fsb09SMGw1b0FHTkRMSGR5MFJkN1pLTjNS%7CImRvd25sb2FkfGRpc2t8YVdROU5qWTROeVpmUFRKNVpYZHZOMkZzYjA5U01HdzFiMEZIVGtSTVNHUjVNRkprTjFwTFRqTlN8OWRkOTBlZDVhNThjY2M0MWFmODFmNWYwMDQzNzM5ZGIi.Lup1vDbibL6twiCPfCMFnLSoDLleNX0cfMHGv5PFaJw%3D",
        "DETAIL_URL": "https://your-domain.bitrix24.com/company/personal/user/1/disk/file/Created files/New test folder/ava555.jpg"
    }
}
```

## 2. Attaching the File to the Task

To attach the file to the task, we use the [tasks.task.files.attach](../../api-reference/tasks/tasks-task-files-attach.md) method with the following parameters:

* `taskId` — the ID of the task. To obtain the ID value, use the [tasks.task.list](../../api-reference/tasks/tasks-task-list.md) method.
* `fileId` — specify the file ID from the result of the previous method `6687`.

{% list tabs %}

- JS

    ```javascript
    BX24.callMethod(
        'tasks.task.files.attach',
        {
            taskId: 3709,
            fileId: 6687
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'tasks.task.files.attach',
        [
            'taskId' => 3709,
            'fileId' => 6687
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

We uploaded the file to the task and received the ID of the link between the disk file and the task `423` in response. To verify the attachment of the file to the task by the link ID, we use the [disk.attachedObject.get](../../api-reference/disk/attached-object/disk-attached-object-get.md) method.

```json
{
    "result": {
        "attachmentId": 423
    },
    "time": {
        "start": 1730795703.5871601,
        "finish": 1730795703.8165951,
        "duration": 0.22943496704101562,
        "processing": 0.18604612350463867,
        "date_start": "2024-11-05T11:35:03+02:00",
        "date_finish": "2024-11-05T11:35:03+02:00",
        "operating_reset_at": 1730796303,
        "operating": 0.18602108955383301
    }
}
```

## Code Example

{% list tabs %}

- JS

    ```javascript
    // Function to upload a file
    function uploadFileToDisk() {
        // ID of the folder where you want to upload the file
        var folderId = 'your_folder_ID';
        // File name and its content in Base64 format
        var fileName = 'your_file_name';
        var fileContentBase64 = 'your_file_content_Base64';
        // ID of the task to which the file needs to be attached
        var taskId = 'your_task_ID';

        // Call the disk.folder.uploadfile method
        BX24.callMethod(
            'disk.folder.uploadfile',
            {
                id: folderId,
                data: {
                    NAME: fileName
                },
                fileContent: [
                    fileName,
                    fileContentBase64
                ]
            },
            function(result) {
                if (result.error()) {
                    console.error('Error uploading file:', result.error());
                } else {
                    console.log('File successfully uploaded!', result.data());
                    var fileId = result.data().ID; // Use ID from the result
                    attachFileToTask(taskId, fileId);
                }
            }
        );
    }

    // Function to attach a file to an existing task
    function attachFileToTask(taskId, fileId) {
        // Call the tasks.task.files.attach method
        BX24.callMethod(
            'tasks.task.files.attach',
            {
                taskId: taskId,
                fileIds: [fileId]
            },
            function(result) {
                if (result.error()) {
                    console.error('Error attaching file to task:', result.error());
                } else {
                    console.log('File successfully attached to the task!', result.data());
                }
            }
        );
    }

    // Call the function to upload the file and attach it to the task
    uploadFileToDisk();
    ```

- PHP

    ```php
    require_once('crest.php');

    // Function to upload a file
    function uploadFileToDisk() {
        // ID of the folder where you want to upload the file
        $folderId = 'your_folder_ID';
        // Name of the file you want to upload
        $fileName = 'your_file_name';
        // Path to the file on your file system
        $filePath = '/path/to/your/file';

        // Read the file content and encode it in Base64
        $fileContentBase64 = base64_encode(file_get_contents($filePath));

        // ID of the task to which the file needs to be attached
        $taskId = 'your_task_ID';

        // Call the disk.folder.uploadfile method
        $result = CRest::call(
            'disk.folder.uploadfile',
            [
                'id' => $folderId,
                'data' => [
                    'NAME' => $fileName
                ],
                'fileContent' => [
                    $fileName,
                    $fileContentBase64
                ]
            ]
        );

        if (isset($result['error'])) {
            echo 'Error uploading file: ' . $result['error'];
        } else {
            echo 'File successfully uploaded!';
            $fileId = $result['result']['ID']; // Use ID from the result
            attachFileToTask($taskId, $fileId);
        }
    }

    // Function to attach a file to an existing task
    function attachFileToTask($taskId, $fileId) {
        // Call the tasks.task.files.attach method
        $result = CRest::call(
            'tasks.task.files.attach',
            [
                'taskId' => $taskId,
                'fileIds' => [$fileId]
            ]
        );

        if (isset($result['error'])) {
            echo 'Error attaching file to task: ' . $result['error'];
        } else {
            echo 'File successfully attached to the task!';
        }
    }

    // Call the function to upload the file and attach it to the task
    uploadFileToDisk();
    ```

{% endlist %}

## Continue Learning

* [How to Create a Task with an Attached File](./how-to-create-task-with-file.md)