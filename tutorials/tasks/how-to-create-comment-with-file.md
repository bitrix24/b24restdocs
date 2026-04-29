# How to Create a Comment in a Task and Attach a File

> Scope: [`drive`, `tasks`](../../api-reference/scopes/permissions.md)
>
> Who can execute the method: users with access to the drive and tasks sections

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

In Bitrix24, there are two types of file fields:

* **File.** This field is not linked to the drive; files are uploaded directly through a [Base64 format string](../../api-reference/files/how-to-upload-files.md).
* **File (drive).** This field is linked to the drive, storing the ID of the drive object. The Base64 format is not processed in this field, so the file must first be uploaded to the Bitrix24 drive.

To create a comment in a task and attach a file, we will sequentially execute two methods:

1. [drive.folder.uploadfile](../../api-reference/disk/folder/disk-folder-upload-file.md) — this method uploads a file to the drive.
2. [task.commentitem.add](../../api-reference/tasks/comment-item/task-comment-item-add.md) — this method creates a comment.

## 1. Uploading a File to the Bitrix24 Drive

To upload a file to the drive, we use the method [drive.folder.uploadfile](../../api-reference/disk/folder/disk-folder-upload-file.md) with the following parameters:

* `id` — we will specify the value `1739` — the identifier of the drive folder where we are uploading the file.
* `data` — we will specify the file name `NAME`, which will be the name under which the file is saved on the Bitrix24 drive.
* `fileContent` — we pass the file in the format ['file_name.extension', 'file as a Base64 encoded string'].

Uploading the file to the drive is a necessary step, as the `UF_FORUM_MESSAGE_DOC` field in comments only accepts the IDs of drive files.

{% include [Example Note](../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```javascript
    BX24.callMethod(
        "drive.folder.uploadfile",
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
        'drive.folder.uploadfile',
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

- Python

    ```python
    from b24pysdk import BitrixWebhook, Client


    client = Client(
        BitrixWebhook(
            domain="your-domain.bitrix24.com",
            auth_token="your-webhook-token",
        )
    )

    response = client.disk.folder.uploadfile(
        bitrix_id=1739,
        data={
            "NAME": "file.pdf",
        },
        file_content=[
            "file555.pdf",
            "/9j/4AAQSkZJRgABAQEASABIAAD/2wBDAAQDAwQDAwQEAwQ///+dAYq6YFKoAv/AFnAa6ArKv8AAtFJVppxCEAulxQ2DWgfMR//2Q==",
        ],
    ).response
    ```

{% endlist %}

As a result of uploading the file to the drive, we receive two different file ID values:

* `FILE_ID`: `28073` — the internal file ID value.
* `ID`: `6687` — the drive object ID, which we will use in methods for working with fields of the "file (drive)" type. If we pass the `FILE_ID` value in the request to modify the "file (drive)" field, the file will either not be attached to the task because there is no drive object with that ID, or the wrong file will be attached.

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
        "CREATE_TIME": "2024-11-01T17:00:55+01:00",
        "UPDATE_TIME": "2024-11-01T17:00:55+01:00",
        "DELETE_TIME": null,
        "CREATED_BY": "1",
        "UPDATED_BY": "1",
        "DELETED_BY": null,
        "DOWNLOAD_URL": "https://your-domain.bitrix24.com/rest/download.json?sessid=9dd90ed5a58ccc41af81f5f0043739db&token=disk%7CaWQ9NjY4NyZfPTJ5ZXdvN2Fsb09SMGw1b0FHTkRMSGR5MFJkN1pLTjNS%7CImRvd25sb2FkfGRpc2t8YVdROU5qWTROeVpmUFRKNVpYZHZOMkZzYjA5U01HdzFiMEZIVGtSTVNHUjVNRkprTjFwTFRqTlN8OWRkOTBlZDVhNThjY2M0MWFmODFmNWYwMDQzNzM5ZGIi.Lup1vDbibL6twiCPfCMFnLSoDLleNX0cfMHGv5PFaJw%3D",
        "DETAIL_URL": "https://your-domain.bitrix24.com/company/personal/user/1/drive/file/Created files/New test folder/file.pdf"
    }
}
```

## 2. Creating a Comment and Attaching a File

To create a comment in a task, we use the method [task.commentitem.add](../../api-reference/tasks/comment-item/task-comment-item-add.md) with the following parameters:

* `TASKID` — the ID of the task, a required field. Without the task ID, the comment will not be created. To obtain the task ID, we use the method [tasks.task.list](../../api-reference/tasks/tasks-task-list.md).
* `AUTHOR_ID` — the ID of the comment author. This parameter can be omitted, in which case the author will automatically be the employee from whose account the request is made.
* `POST_MESSAGE` — the text of the comment.
* `UF_FORUM_MESSAGE_DOC` — we will specify the value `n6687`. This is the drive file ID from the result of the previous method, to which we add the prefix `n` for uploading the file to the field.

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

- Python

    ```python
    response = client.task.commentitem.add(
        task_id=3711,
        fields={
            "POST_MESSAGE": "comment for test",
            "AUTHOR_ID": 29,
            "UF_FORUM_MESSAGE_DOC": [
                "n6687",
            ],
        },
    ).response
    ```

{% endlist %}

We created a comment with ID `9393`.

```json
{
    "result": 9393
}
```

In the received result, there is no information about the file attached to the comment. To check if the file was successfully attached, we will execute the method [task.commentitem.get](../../api-reference/tasks/comment-item/task-comment-item-get.md).

## Code Example

{% list tabs %}

- JS

    ```javascript
    // Function to upload a file
    function uploadFileToDrive() {
        // ID of the folder to which the file needs to be uploaded
        var folderId = 'FOLDER_ID';
        // File name and its content in Base64 format
        var fileName = 'file_name';
        var fileContentBase64 = 'file_content_Base64';

        // Call the method drive.folder.uploadfile
        BX24.callMethod(
            'drive.folder.uploadfile',
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
                    createCommentWithFile(fileId);
                }
            }
        );
    }

    // Function to create a comment with a file
    function createCommentWithFile(fileId) {
        // Comment parameters
        var taskID = 'TASK_ID';
        var commentMessage = 'comment_text';
        var authorId = 'AUTHOR_ID';

        // Call the method task.commentitem.add
        BX24.callMethod(
            'task.commentitem.add',
            {
                TASKID: taskID,
                FIELDS: {
                    POST_MESSAGE: commentMessage,
                    AUTHOR_ID: authorId,
                    UF_FORUM_MESSAGE_DOC: ['n' + fileId] // Add prefix 'n' to the file ID
                }
            },
            function(result) {
                if (result.error()) {
                    console.error('Error creating comment:', result.error());
                } else {
                    console.log('Comment successfully created!', result.data());
                }
            }
        );
    }

    // Call the function to upload a file and create a comment
    uploadFileToDrive();
    ```

- PHP

    ```php
    require_once('crest.php');

    // Function to upload a file
    function uploadFileToDrive() {
        // ID of the folder to which the file needs to be uploaded
        $folderId = 'FOLDER_ID';
        // Name of the file you want to upload
        $fileName = 'file_name';
        // Path to the file on your file system
        $filePath = '/path/to/your/file';

        // Read the file content and encode it in Base64
        $fileContentBase64 = base64_encode(file_get_contents($filePath));

        // Call the method drive.folder.uploadfile
        $result = CRest::call(
            'drive.folder.uploadfile',
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
            createCommentWithFile($fileId);
        }
    }

    // Function to create a comment with a file
    function createCommentWithFile($fileId) {
        // Comment parameters
        $taskID = 'TASK_ID';
        $commentMessage = 'comment_text';
        $authorId = 'AUTHOR_ID';

        // Call the method task.commentitem.add
        $result = CRest::call(
            'task.commentitem.add',
            [
                'TASKID' => $taskID,
                'FIELDS' => [
                    'POST_MESSAGE' => $commentMessage,
                    'AUTHOR_ID' => $authorId,
                    'UF_FORUM_MESSAGE_DOC' => ['n' . $fileId] // Add prefix 'n' to the file ID
                ]
            ]
        );

        if (isset($result['error'])) {
            echo 'Error creating comment: ' . $result['error'];
        } else {
            echo 'Comment successfully created!';
        }
    }

    // Call the function to upload a file and create a comment
    uploadFileToDrive();
    ```

- Python

    ```python
    from b24pysdk import BitrixWebhook, Client
    from b24pysdk.errors import BitrixAPIError


    def upload_file_to_drive(client):
        folder_id = 1739
        file_name = "file_name"
        file_content_base64 = "file_content_Base64"

        try:
            result = client.disk.folder.uploadfile(
                bitrix_id=folder_id,
                data={"NAME": file_name},
                file_content=[file_name, file_content_base64],
            ).response.result
        except BitrixAPIError as error:
            print(f"Error uploading file: {error}")
        else:
            print("File successfully uploaded!")
            file_id = result["ID"]
            create_comment_with_file(client, file_id)


    def create_comment_with_file(client, file_id):
        task_id = "TASK_ID"
        comment_message = "comment_text"
        author_id = "AUTHOR_ID"

        try:
            client.task.commentitem.add(
                task_id=task_id,
                fields={
                    "POST_MESSAGE": comment_message,
                    "AUTHOR_ID": author_id,
                    "UF_FORUM_MESSAGE_DOC": [f"n{file_id}"],
                },
            ).response
        except BitrixAPIError as error:
            print(f"Error creating comment: {error}")
        else:
            print("Comment successfully created!")


    client = Client(
        BitrixWebhook(
            domain="your-domain.bitrix24.com",
            auth_token="your-webhook-token",
        )
    )

    upload_file_to_drive(client)
    ```

{% endlist %}