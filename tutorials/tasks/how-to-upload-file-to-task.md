# How to Upload a File to a Task

> Scope: [`disk`, `tasks`](../../api-reference/scopes/permissions.md)
>
> Who can execute the method: users with access to the Drive and Task sections

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Bitrix24 has two types of file fields: 

* **File.** This field is not linked to Drive; files are uploaded directly into it via a [Base64 format string](../../api-reference/files/how-to-upload-files.md)
* **File (Drive).** This field is linked to Drive and stores the Drive object ID. Base64 format is not processed in this field, so you must first upload the file to Bitrix24 Drive

To attach a file to a task, execute these two methods in sequence:

1. [disk.folder.uploadfile](../../api-reference/disk/folder/disk-folder-upload-file.md) — uploads a file to Drive
2. [tasks.task.files.attach](../../api-reference/tasks/tasks-task-files-attach.md) — attaches a Drive file to a task
   
## 1. Uploading a File to Bitrix24 Drive

To upload a file to Drive, use the [disk.folder.uploadfile](../../api-reference/disk/folder/disk-folder-upload-file.md) method with the following parameters:

* `id` — specify the value of `1739` — the identifier of the Drive folder where the file will be uploaded
* `data` — specify the filename `NAME`; the file will be saved on Bitrix24 Drive with this name
* `fileContent` — pass the file in the format `['filename.extension', 'file as a Base64 encoded string']`

Uploading a file to Drive is a required step because the `UF_TASK_WEBDAV_FILES` field in tasks only accepts Drive file IDs.

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
                NAME: 'ava555.jpg'
            },
            fileContent: [
                'avatar.jpg',
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
        ['NAME' => 'ava555.jpg'],
        [
            'avatar.jpg',
            '/9j/4AAQSkZJRgABAQEASABIAAD/2wBDAAQDAwQDAwQEAwQ///+dAYq6YFKoAv/AFnAa6ArKv8AAtFJVppxCEAulxQ2DWgfMR//2Q=='
        ]
    );

    echo '<PRE>';
    print_r($result->getFile());
    echo '</PRE>';
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

    result = client.disk.folder.uploadfile(
        bitrix_id=1739,
        data={
            "NAME": "ava555.jpg",
        },
        file_content=[
            "avatar.jpg",
            "/9j/4AAQSkZJRgABAQEASABIAAD/2wBDAAQDAwQDAwQEAwQ///+dAYq6YFKoAv/AFnAa6ArKv8AAtFJVppxCEAulxQ2DWgfMR//2Q==",
        ],
    ).response.result
    ```

{% endlist %}

As a result of uploading the file to Drive, you will receive two different file ID values:

* `FILE_ID`: `28073` — the internal file ID value
* `ID`: `6687` — the Drive object ID; use this value in methods when working with "File (Drive)" type fields
If you pass the value `FILE_ID` in a request to change a "File (Drive)" field, the file will either not be attached to the task because no Drive object exists with that ID, or the wrong file will be attached

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
        "CREATE_TIME": "2024-11-01T17:00:55+03:00",
        "UPDATE_TIME": "2024-11-01T17:00:55+03:00",
        "DELETE_TIME": null,
        "CREATED_BY": "1",
        "UPDATED_BY": "1",
        "DELETED_BY": null,
        "DOWNLOAD_URL": "https://your-domain.bitrix24.com/rest/download.json?sessid=9dd90ed5a58ccc41af81f5f0043739db&token=disk%7CaWQ9NjY4NyZfPTJ5ZXdvN2Fsb09SMGw1b0FHTkRMSGR5MFJkN1pLTjNS%7CImRvd25sb2FkfGRpc2t8YVdROU5qWTROeVpmUFRKNVpYZHZOMkZzYjA5U01HdzFiMEZIVGtSTVNHUjVNRkprTjFwTFRqTlN8OWRkOTBlZDVhNThjY2M0MWFmODFmNWYwMDQzNzM5ZGIi.Lup1vDbibL6twiCPfCMFnLSoDLleNX0cfMHGv5PFaJw%3D",
        "DETAIL_URL": "https://your-domain.bitrix24.com/company/personal/user/1/disk/file/Created files/New folder for process test/ava555.jpg"
    }
}
```
## 2. Attaching a File to a Task

To attach a file to a task, use the [tasks.task.files.attach](../../api-reference/tasks/tasks-task-files-attach.md) method with the following parameters:

* `taskId` — the task ID. To retrieve the ID value, use the [tasks.task.list](../../api-reference/tasks/tasks-task-list.md) method
* `fileId` — specify the file ID from the result of the previous method `6687`

{% list tabs %}

- JS

    ```javascript
    const response = await $b24.actions.v2.call.make({
        method: 'tasks.task.files.attach',
        params: {
            taskId: 3709,
            fileId: 6687
        },
        requestId: 'task-files-attach'
    })

    if (!response.isSuccess) {
        throw new Error(response.getErrorMessages().join('; '))
    }

    const result = response.getData().result
    ```

- PHP

    ```php
    $result = $serviceBuilder->core->call(
        'tasks.task.files.attach',
        [
            'taskId' => 3709,
            'fileId' => 6687
        ]
    )->getResponseData()->getResult();

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Python

    ```python
    result = client.tasks.task.files.attach(
        task_id=3709,
        file_id=6687,
    ).response.result
    ```

{% endlist %}

We have uploaded the file to the task and received the link ID between the Drive file and the task in the response: `423`. To verify the file attachment to a task using the link ID, use the [disk.attachedObject.get](../../api-reference/disk/attached-object/disk-attached-object-get.md) method. 

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
        "date_start": "2024-11-05T11:35:03+03:00",
        "date_finish": "2024-11-05T11:35:03+03:00",
        "operating_reset_at": 1730796303,
        "operating": 0.18602108955383301
    }
}
```

## Code Example

{% list tabs %}

- JS

    ```javascript
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/')

    // Function for uploading a file
    async function uploadFileToDisk() {
        // ID of the folder where you want to upload the file
        const folderId = 'your_folder_ID';
        // File name and its content in Base64 format
        const fileName = 'your_file_name';
        const fileContentBase64 = 'your_file_content_Base64';
        // ID of the task to which the file should be attached
        const taskId = 'your_task_ID';

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
        await attachFileToTask(taskId, fileId);
    }

    // Function for attaching a file to an existing task
    async function attachFileToTask(taskId, fileId) {
        // Calling the tasks.task.files.attach method
        const response = await $b24.actions.v2.call.make({
            method: 'tasks.task.files.attach',
            params: {
                taskId: taskId,
                fileId: fileId
            },
            requestId: 'task-files-attach'
        });

        if (!response.isSuccess) {
            console.error('Error while attaching the file to the task:', response.getErrorMessages().join('; '));
            return;
        }

        console.log('File successfully attached to the task!', response.getData().result);
    }

    // Calling the function to upload a file and attach it to a task
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
        // ID of the folder where you want to upload the file
        $folderId = 'your_folder_ID';
        // The name of the file you want to upload
        $fileName = 'your_file_name';
        // Path to the file on your file system
        $filePath = '/path/to/your/file';

        // Reading the file content and encoding it in Base64
        $fileContentBase64 = base64_encode(file_get_contents($filePath));

        // ID of the task to which the file should be attached
        $taskId = 'your_task_ID';

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
        attachFileToTask($serviceBuilder, $taskId, $fileId);
    }

    // Function for attaching a file to an existing task
    function attachFileToTask($serviceBuilder, $taskId, $fileId) {
        // Calling the tasks.task.files.attach method
        try {
            $serviceBuilder->core->call(
                'tasks.task.files.attach',
                [
                    'taskId' => $taskId,
                    'fileId' => $fileId
                ]
            );
        } catch (BaseException $e) {
            echo 'Error while attaching the file to the task: ' . $e->getMessage();
            return;
        }

        echo 'File successfully attached to the task!';
    }

    // Calling the function to upload a file and attach it to a task
    uploadFileToDisk($serviceBuilder);
    ```

- Python

    ```python
    from b24pysdk import BitrixWebhook, Client
    from b24pysdk.errors import BitrixAPIError

    def upload_file_to_disk(client):
        folder_id = "your_folder_ID"
        file_name = "your_file_name"
        file_content_base64 = "your_file_content_Base64"
        task_id = "your_task_ID"

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
            attach_file_to_task(client, task_id, file_id)

    def attach_file_to_task(client, task_id, file_id):
        try:
            client.tasks.task.files.attach(
                task_id=task_id,
                file_id=file_id,
            ).response
        except BitrixAPIError as error:
            print(f"File attachment error to the task: {error}")
        else:
            print("File successfully attached to the task!")

    client = Client(
        BitrixWebhook(
            domain="your-domain.bitrix24.com",
            webhook_token="user_id/webhook_key",
        )
    )

    upload_file_to_disk(client)
    ```

{% endlist %}

## Continue Learning

* [How to Create a Task with an Attached File](./how-to-create-task-with-file.md)
