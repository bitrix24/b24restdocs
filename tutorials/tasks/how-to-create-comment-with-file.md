# How to Create a Comment in a Task and Attach a File

> Scope: [`drive`, `tasks`](../../api-reference/scopes/permissions.md)
>
> Who can execute the method: users with access to the drive and tasks sections

In Bitrix24, there are two types of file fields:

* **File.** The field is not linked to the drive; files are uploaded directly through the [Base64 format string](../../api-reference/bx24-js-sdk/how-to-call-rest-methods/files.md).
* **File (drive).** The field is linked to the drive, and it stores the ID of the drive object. The Base64 format is not processed in this field, so the file must first be uploaded to the Bitrix24 drive.

To create a comment in a task and attach a file, we will sequentially execute two methods:

1. [disk.folder.uploadfile](../../api-reference/disk/folder/disk-folder-upload-file.md) — this method uploads a file to the drive.
2. [task.commentitem.add](../../api-reference/tasks/comment-item/task-comment-item-add.md) — this method creates a comment.

## 1. Uploading a File to the Bitrix24 Drive

To upload a file to the drive, we use the method [disk.folder.uploadfile](../../api-reference/disk/folder/disk-folder-upload-file.md) with the following parameters:

* `id` — we specify the value `1739` — the identifier of the drive folder where we are uploading the file.
* `data` — we specify the file name `NAME`, under which the file will be saved on the Bitrix24 drive.
* `fileContent` — we pass the file in the format ['file_name.extension', 'file as a Base64 encoded string'].

Uploading the file to the drive is a necessary step, as the `UF_FORUM_MESSAGE_DOC` field in comments only accepts the IDs of drive files.

{% include [Footnote on examples](../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```javascript
    BX24.callMethod(
        "disk.folder.uploadfile",
        {
            id: 1739,
            data: {
                NAME: "file.pdf"
            },
            fileContent: [
                'file555.pdf',
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
                'NAME' => 'file.pdf'
            ],
            'fileContent' => [
                'file555.pdf',
                '/9j/4AAQSkZJRgABAQEASABIAAD/2wBDAAQDAwQDAwQEAwQ///+dAYq6YFKoAv/AFnAa6ArKv8AAtFJVppxCEAulxQ2DWgfMR//2Q=='
            ]
        ]
    );
    ```

{% endlist %}

As a result of uploading the file to the drive, we received two different file ID values:

* `FILE_ID`: `28073` — the internal file ID value.
* `ID`: `6687` — the ID of the drive object; this value is used in methods for working with fields of the "file (drive)" type. If the request to change the "file (drive)" field passes the `FILE_ID` value, the file will either not be attached to the task because there is no drive object with that ID, or the wrong file will be attached.

```json
{
    "result": {
        "ID": 6687,
        "NAME": "file.pdf",
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
        "DETAIL_URL": "https://your-domain.bitrix24.com/company/personal/user/1/disk/file/Created files/New test folder/file.pdf"
    }
}
```

## 2. Creating a Comment and Attaching a File

To create a comment in a task, we use the method [task.commentitem.add](../../api-reference/tasks/comment-item/task-comment-item-add.md) with the following parameters:

* `TASKID` — the ID of the task, a required field. Without the task ID, the comment will not be created. To get the task ID, we use the method [tasks.task.list](../../api-reference/tasks/tasks-task-list.md).
* `AUTHOR_ID` — the ID of the comment author. This parameter can be omitted, in which case the author will automatically be the employee from whose account the request is made.
* `POST_MESSAGE` — the text of the comment.
* `UF_FORUM_MESSAGE_DOC` — we specify the value `n6687`. This is the ID of the drive file from the previous method's result, to which we add the prefix `n` for uploading the file to the field.

{% list tabs %}

- JS

    ```javascript
    BX24.callMethod(
        'task.commentitem.add',
        {
            TASKID: 3711,
            FIELDS: {
                POST_MESSAGE: 'comment for test',
                AUTHOR_ID: 29,
                UF_FORUM_MESSAGE_DOC: [
                    "n6687"
                ]
            }
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'task.commentitem.add',
        [
            'TASKID' => 3711,
            'FIELDS' => [
                'POST_MESSAGE' => 'comment for test',
                'AUTHOR_ID' => 29,
                'UF_FORUM_MESSAGE_DOC' => [
                    'n6687'
                ]
            ]
        ]
    );
    ```

{% endlist %}

We created a comment with ID `9393`.

```json
{
    "result": 9393
}
```

The received result does not contain information about the file attached to the comment. To check if the file was successfully attached, we will execute the method [task.commentitem.get](../../api-reference/tasks/comment-item/task-comment-item-get.md).

## Code Example

{% list tabs %}

- JS

    ```javascript
    // Function to upload a file
    function uploadFileToDisk() {
        // ID of the folder to which the file needs to be uploaded
        var folderId = 'folder_ID';
        // File name and its content in Base64 format
        var fileName = 'file_name';
        var fileContentBase64 = 'file_content_Base64';

        // Calling the disk.folder.uploadfile method
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
                    console.log('File uploaded successfully!', result.data());
                    var fileId = result.data().ID; // Using ID from the result
                    createCommentWithFile(fileId);
                }
            }
        );
    }

    // Function to create a comment with a file
    function createCommentWithFile(fileId) {
        // Comment parameters
        var taskID = 'task_ID';
        var commentMessage = 'comment_text';
        var authorId = 'comment_author_ID';

        // Calling the task.commentitem.add method
        BX24.callMethod(
            'task.commentitem.add',
            {
                TASKID: taskID,
                FIELDS: {
                    POST_MESSAGE: commentMessage,
                    AUTHOR_ID: authorId,
                    UF_FORUM_MESSAGE_DOC: ['n' + fileId] // Adding prefix 'n' to the file ID
                }
            },
            function(result) {
                if (result.error()) {
                    console.error('Error creating comment:', result.error());
                } else {
                    console.log('Comment created successfully!', result.data());
                }
            }
        );
    }

    // Calling the function to upload a file and create a comment
    uploadFileToDisk();
    ```

- PHP

    ```php
    require_once('crest.php');

    // Function to upload a file
    function uploadFileToDisk() {
        // ID of the folder to which the file needs to be uploaded
        $folderId = 'folder_ID';
        // Name of the file you want to upload
        $fileName = 'file_name';
        // Path to the file on your file system
        $filePath = '/path/to/your/file';

        // Reading the file content and encoding it in Base64
        $fileContentBase64 = base64_encode(file_get_contents($filePath));

        // Calling the disk.folder.uploadfile method
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
            echo 'File uploaded successfully!';
            $fileId = $result['result']['ID']; // Using ID from the result
            createCommentWithFile($fileId);
        }
    }

    // Function to create a comment with a file
    function createCommentWithFile($fileId) {
        // Comment parameters
        $taskID = 'task_ID';
        $commentMessage = 'comment_text';
        $authorId = 'comment_author_ID';

        // Calling the task.commentitem.add method
        $result = CRest::call(
            'task.commentitem.add',
            [
                'TASKID' => $taskID,
                'FIELDS' => [
                    'POST_MESSAGE' => $commentMessage,
                    'AUTHOR_ID' => $authorId,
                    'UF_FORUM_MESSAGE_DOC' => ['n' . $fileId] // Adding prefix 'n' to the file ID
                ]
            ]
        );

        if (isset($result['error'])) {
            echo 'Error creating comment: ' . $result['error'];
        } else {
            echo 'Comment created successfully!';
        }
    }

    // Calling the function to upload a file and create a comment
    uploadFileToDisk();
    ```

{% endlist %}