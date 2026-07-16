# How to Create a Comment in a Task and Attach a File

> Scope: [`disk`, `tasks`](../../api-reference/scopes/permissions.md)
>
> Who can execute the method: users with access to Drive and Task sections

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Bitrix24 has two types of file fields: 

* **File.** This field is not linked to Drive; files are uploaded directly via a [Base64 format string](../../api-reference/files/how-to-upload-files.md)
* **File (Drive).** This field is linked to Drive and stores the Drive object ID. Base64 format is not processed in this field, so the file must first be uploaded to Bitrix24 Drive

Task comments are stored in the Task chat. To create a comment with a file, perform the following methods sequentially:

1. [disk.folder.uploadfile](../../api-reference/disk/folder/disk-folder-upload-file.md) — uploads a file to Drive
2. [tasks.task.get](../../api-reference/tasks/tasks-task-get.md) — returns the `chatId` of the Task chat
3. [im.disk.file.commit](../../api-reference/chats/files/im-disk-file-commit.md) — attaches a Drive file to the Task chat along with the comment text

## 1. Uploading a File to Bitrix24 Drive

To upload a file to Drive, use the [disk.folder.uploadfile](../../api-reference/disk/folder/disk-folder-upload-file.md) method with the following parameters:

* `id` — specify the `1739` value — the identifier of the Drive folder where the file is being uploaded
* `data` — specify the filename `NAME`; the file will be saved on Bitrix24 Drive with this name
* `fileContent` — pass the file in the format `['filename.extension', 'file as a Base64 encoded string']`

Uploading a file to Drive is a required step because the [im.disk.file.commit](../../api-reference/chats/files/im-disk-file-commit.md) method only attaches files to a comment that have already been uploaded to Bitrix24 Drive.

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```javascript
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/')

    const response = await $b24.actions.v2.call.make({
        method: 'disk.folder.uploadfile',
        params: {
            id: 1739,
            data: {
                NAME: 'file.pdf'
            },
            fileContent: [
                'file555.pdf',
                '/9j/4AAQSkZJRgABAQEASABIAAD/2wBDAAQDAwQDAwQEAwQ///+dAYq6YFKoAv/AFnAa6ArKv8AAtFJVppxCEAulxQ2DWgfMR//2Q=='
            ]
        },
        requestId: 'disk-uploadfile'
    })

    if (!response.isSuccess) {
        throw new Error(response.getErrorMessages().join('; '))
    }

    const result = response.getData().result
    ```

- PHP

    ```php
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Symfony\Component\EventDispatcher\EventDispatcher;

    $serviceBuilder = (new ServiceBuilderFactory(new EventDispatcher(), $log))
        ->initFromWebhook('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/');

    $result = $serviceBuilder->getDiskScope()->folder()->uploadFile(
        1739,
        ['NAME' => 'file.pdf'],
        [
            'file555.pdf',
            '/9j/4AAQSkZJRgABAQEASABIAAD/2wBDAAQDAwQDAwQEAwQ///+dAYq6YFKoAv/AFnAa6ArKv8AAtFJVppxCEAulxQ2DWgfMR//2Q=='
        ]
    );
    ```

- Python

    ```python
    from b24pysdk import BitrixWebhook, Client

    client = Client(
        BitrixWebhook(
            domain="your-domain.bitrix24.com",
            webhook_token="user_id/webhook_key",
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

As a result of uploading the file to Drive, you receive two different file ID values:

* `FILE_ID`: `28073` — the internal file ID value
* `ID`: `6687` — the Drive object ID; use this value in methods when working with "File (Drive)" type fields
If you pass the `FILE_ID` value in a request to change a "File (Drive)" field, the file will either not be attached to the task because no Drive object exists with that ID, or the wrong file will be attached

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
        "CREATE_TIME": "2024-11-01T17:00:55+03:00",
        "UPDATE_TIME": "2024-11-01T17:00:55+03:00",
        "DELETE_TIME": null,
        "CREATED_BY": "1",
        "UPDATED_BY": "1",
        "DELETED_BY": null,
        "DOWNLOAD_URL": "https://your-domain.bitrix24.com/rest/download.json?sessid=9dd90ed5a58ccc41af81f5f0043739db&token=disk%7CaWQ9NjY4NyZfPTJ5ZXdvN2Fsb09SMGw1b0FHTkRMSGR5MFJkN1pLTjNS%7CImRvd25sb2FkfGRpc2t8YVdROU5qWTROeVpmUFRKNVpYZHZOMkZzYjA5U01HdzFiMEZIVGtSTVNHUjVNRkprTjFwTFRqTlN8OWRkOTBlZDVhNThjY2M0MWFmODFmNWYwMDQzNzM5ZGIi.Lup1vDbibL6twiCPfCMFnLSoDLleNX0cfMHGv5PFaJw%3D",
        "DETAIL_URL": "https://your-domain.bitrix24.com/company/personal/user/1/disk/file/Created files/New folder for process test/file.pdf"
    }
}
```

## 2. Retrieving the Task Chat chatId

To attach a file to a comment, you need the Task chat identifier. Retrieve it using the [tasks.task.get](../../api-reference/tasks/tasks-task-get.md) method with the following parameters:

* `taskId` — the Task ID. To retrieve a Task ID, use the [tasks.task.list](../../api-reference/tasks/tasks-task-list.md) method
* `select` — specify the `CHAT_ID` field; the [tasks.task.get](../../api-reference/tasks/tasks-task-get.md) method will not return the chat identifier without `CHAT_ID` in `select`

{% list tabs %}

- JS

    ```javascript
    const response = await $b24.actions.v2.call.make({
        method: 'tasks.task.get',
        params: {
            taskId: 3711,
            select: ['ID', 'CHAT_ID']
        },
        requestId: 'task-get'
    })

    if (!response.isSuccess) {
        throw new Error(response.getErrorMessages().join('; '))
    }

    const chatId = response.getData().result.task.chatId
    ```

- PHP

    ```php
    $task = $serviceBuilder->core->call(
        'tasks.task.get',
        [
            'taskId' => 3711,
            'select' => ['ID', 'CHAT_ID']
        ]
    )->getResponseData()->getResult()['task'];

    $chatId = $task['chatId'];
    ```

- Python

    ```python
    task = client.tasks.task.get(
        bitrix_id=3711,
        select=["ID", "CHAT_ID"],
    ).response.result["task"]

    chat_id = task["chatId"]
    ```

{% endlist %}

As a result, you receive the `chatId` of the Task chat.

```json
{
    "result": {
        "task": {
            "id": "3711",
            "chatId": 861
        }
    }
}
```

## 3. Create a Comment with a File

The [im.disk.file.commit](../../api-reference/chats/files/im-disk-file-commit.md) method adds a Drive file to the task chat as a separate message — this is the comment with a file. Use the following parameters:

* `CHAT_ID` — the `chatId` of the task chat from the previous method's result
* `FILE_ID` — the ID of the Drive object `6687` from the [disk.folder.uploadfile](../../api-reference/disk/folder/disk-folder-upload-file.md) method result
* `MESSAGE` — the comment text that will be sent along with the file

{% list tabs %}

- JS

    ```javascript
    const response = await $b24.actions.v2.call.make({
        method: 'im.disk.file.commit',
        params: {
            CHAT_ID: 861,
            FILE_ID: 6687,
            MESSAGE: 'comment for test'
        },
        requestId: 'im-disk-file-commit'
    })

    if (!response.isSuccess) {
        throw new Error(response.getErrorMessages().join('; '))
    }

    const result = response.getData().result
    ```

- PHP

    ```php
    $result = $serviceBuilder->getIMScope()->disk()->commitFile(
        chatId: 861,
        fileId: 6687,
        message: 'comment for test'
    );
    ```

- Python

    ```python
    token = BitrixWebhook(
        domain="your-domain.bitrix24.com",
        webhook_token="user_id/webhook_key",
    )

    result = token.call_method(
        "im.disk.file.commit",
        {
            "CHAT_ID": 861,
            "FILE_ID": 6687,
            "MESSAGE": "comment for test",
        },
    )
    ```

{% endlist %}

The comment with a file has been created. The method returns the `MESSAGE_ID` of the message in the task chat and the `DISK_ID` of the file added to the chat.

```json
{
    "result": {
        "FILES": {
            "disk1899": {
                "id": 1903,
                "chatId": 861,
                "type": "file",
                "name": "file.pdf",
                "extension": "pdf",
                "size": 70,
                "status": "done",
                "authorId": 1
            }
        },
        "DISK_ID": [
            1903
        ],
        "MESSAGE_ID": 6175
    }
}
```

The `MESSAGE_ID` field is the identifier of the message with the file in the task chat, and `DISK_ID` is the identifier of the file in the chat. The comment with a file is displayed in the task chat.

## Code Example

{% list tabs %}

- JS

    ```javascript
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/')

    // Function for uploading a file
    async function uploadFileToDisk() {
        // Folder ID where the file needs to be uploaded
        const folderId = 'folder_ID';
        // File name and its content in Base64 format
        const fileName = 'file_name';
        const fileContentBase64 = 'file_content_Base64';

        // Calling the disk.folder.uploadfile method
        const response = await $b24.actions.v2.call.make({
            method: 'disk.folder.uploadfile',
            params: {
                id: folderId,
                data: {
                    NAME: fileName
                },
                fileContent: [
                    fileName,
                    fileContentBase64
                ]
            },
            requestId: 'disk-uploadfile'
        });

        if (!response.isSuccess) {
            console.error('Error while uploading the file:', response.getErrorMessages().join('; '));
            return;
        }

        console.log('File uploaded successfully!', response.getData().result);
        const fileId = response.getData().result.ID; // Using the ID from the result
        await createCommentWithFile(fileId);
    }

    // Function for creating a comment with a file
    async function createCommentWithFile(fileId) {
        // Comment parameters
        const taskID = 'task_ID';
        const commentMessage = 'comment_text';

        // Getting the task chat's chatId
        const taskResponse = await $b24.actions.v2.call.make({
            method: 'tasks.task.get',
            params: {
                taskId: taskID,
                select: ['ID', 'CHAT_ID']
            },
            requestId: 'task-get'
        });

        if (!taskResponse.isSuccess) {
            console.error('Error while getting the task:', taskResponse.getErrorMessages().join('; '));
            return;
        }

        const chatId = taskResponse.getData().result.task.chatId;

        // Attaching the file to the task chat along with the comment text
        const fileResponse = await $b24.actions.v2.call.make({
            method: 'im.disk.file.commit',
            params: {
                CHAT_ID: chatId,
                FILE_ID: fileId,
                MESSAGE: commentMessage
            },
            requestId: 'im-disk-file-commit'
        });

        if (!fileResponse.isSuccess) {
            console.error('Error while creating a comment:', fileResponse.getErrorMessages().join('; '));
            return;
        }

        console.log('Comment with file created successfully!', fileResponse.getData().result);
    }

    // Calling the function to upload a file and create a comment
    await uploadFileToDisk();

    $b24.destroy();
    ```

- PHP

    ```php
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Bitrix24\SDK\Core\Exceptions\BaseException;
    use Symfony\Component\EventDispatcher\EventDispatcher;

    $serviceBuilder = (new ServiceBuilderFactory(new EventDispatcher(), $log))
        ->initFromWebhook('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/');

    // Function for uploading a file
    function uploadFileToDisk($serviceBuilder) {
        // Folder ID where the file needs to be uploaded
        $folderId = 'folder_ID';
        // The name of the file you want to upload
        $fileName = 'file_name';
        // Path to the file on your file system
        $filePath = '/path/to/your/file';

        // Reading the file content and encoding it in Base64
        $fileContentBase64 = base64_encode(file_get_contents($filePath));

        // Calling the disk.folder.uploadfile method
        try {
            $result = $serviceBuilder->getDiskScope()->folder()->uploadFile(
                (int)$folderId,
                ['NAME' => $fileName],
                [
                    $fileName,
                    $fileContentBase64
                ]
            );
        } catch (BaseException $e) {
            echo 'Error while uploading the file: ' . $e->getMessage();
            return;
        }

        echo 'File uploaded successfully!';
        $fileId = $result->getId(); // Using the ID from the result
        createCommentWithFile($serviceBuilder, $fileId);
    }

    // Function for creating a comment with a file
    function createCommentWithFile($serviceBuilder, $fileId) {
        // Comment parameters
        $taskID = 'task_ID';
        $commentMessage = 'comment_text';

        // Getting the task chat's chatId
        try {
            $task = $serviceBuilder->core->call(
                'tasks.task.get',
                [
                    'taskId' => $taskID,
                    'select' => ['ID', 'CHAT_ID']
                ]
            )->getResponseData()->getResult()['task'];
        } catch (BaseException $e) {
            echo 'Error while getting the task: ' . $e->getMessage();
            return;
        }

        $chatId = $task['chatId'];

        // Attaching the file to the task chat along with the comment text
        try {
            $serviceBuilder->getIMScope()->disk()->commitFile(
                chatId: (int)$chatId,
                fileId: $fileId,
                message: $commentMessage
            );
        } catch (BaseException $e) {
            echo 'Error while creating a comment: ' . $e->getMessage();
            return;
        }

        echo 'Comment with file created successfully!';
    }

    // Calling the function to upload a file and create a comment
    uploadFileToDisk($serviceBuilder);
    ```

- Python

    ```python
    from b24pysdk import BitrixWebhook, Client
    from b24pysdk.errors import BitrixAPIError

    webhook = BitrixWebhook(
        domain="your-domain.bitrix24.com",
        webhook_token="user_id/webhook_key",
    )
    client = Client(webhook)

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
            print(f"File upload error: {error}")
        else:
            print("File uploaded successfully!")
            file_id = result["ID"]
            create_comment_with_file(client, file_id)

    def create_comment_with_file(client, file_id):
        task_id = "task_ID"
        comment_message = "comment_text"

        # Getting the task chat's chatId
        try:
            task = client.tasks.task.get(
                bitrix_id=task_id,
                select=["ID", "CHAT_ID"],
            ).response.result["task"]
        except BitrixAPIError as error:
            print(f"Task retrieval error: {error}")
            return

        chat_id = task["chatId"]

        # Attaching the file to the task chat along with the comment text
        try:
            webhook.call_method(
                "im.disk.file.commit",
                {
                    "CHAT_ID": chat_id,
                    "FILE_ID": file_id,
                    "MESSAGE": comment_message,
                },
            )
        except BitrixAPIError as error:
            print(f"Comment creation error: {error}")
        else:
            print("Comment with file created successfully!")

    upload_file_to_drive(client)
    ```

{% endlist %}
