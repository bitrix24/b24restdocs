# How to Upload a File to a Task

> Scope: [`disk`](../../api-reference/scopes/permissions.md), [`task`](../../api-reference/scopes/permissions.md)
>
> Who can execute the methods: to complete the whole scenario, you need permission to add files to a Drive folder, edit the task, and read the file
>
> - [disk.folder.uploadFile](../../api-reference/disk/folder/disk-folder-upload-file.md) — a user with the Add permission for the Drive folder
> - [tasks.task.files.attach](../../api-reference/tasks/tasks-task-files-attach.md) — the task creator or a user with permission to edit the task and read the file
> - [disk.attachedObject.get](../../api-reference/disk/attached-object/disk-attached-object-get.md) — a user with the Read permission for the file

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Bitrix24 has two types of file fields:

- **File.** This field is not linked to Drive. Files are uploaded directly through a [Base64 format string](../../api-reference/files/how-to-upload-files.md)
- **File (Drive).** This field is linked to Drive. The field stores the Drive object ID. Base64 format is not processed in this field, so the file must first be uploaded to Bitrix24 Drive

To attach a file to a task, perform these two methods in sequence:

1. [disk.folder.uploadFile](../../api-reference/disk/folder/disk-folder-upload-file.md) — uploads a file to Drive
2. [tasks.task.files.attach](../../api-reference/tasks/tasks-task-files-attach.md) — attaches a Drive file to a task

## 1. Upload the File to Bitrix24 Drive

Use the [disk.folder.uploadFile](../../api-reference/disk/folder/disk-folder-upload-file.md) method with the following parameters:

- `id` — specify the value `1739`, the identifier of the Drive folder where the file is uploaded
- `data` — specify the file name in `NAME`. The file will be saved in Bitrix24 Drive with this name
- `fileContent` — pass the file in the format `['file_name.extension', 'file as a Base64-encoded string']`

Uploading the file to Drive is required because the `UF_TASK_WEBDAV_FILES` field in tasks accepts only Drive file IDs.

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```javascript
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    const response = await $b24.actions.v2.call.make({
        method: 'disk.folder.uploadFile',
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

- Python

    ```python
    from b24pysdk import BitrixWebhook, Client

    token = BitrixWebhook(
        domain="your-domain.bitrix24.com",
        webhook_token="user_id/webhook_key",
    )
    client = Client(token)

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
{% endlist %}

As a result of uploading the file to Drive, you get two different file ID values:

- `FILE_ID`: `28073` — the internal file ID
- `ID`: `6687` — the Drive object ID. Use this value when working with File (Drive) fields

If you pass `FILE_ID` instead of `ID` in a request that updates a File (Drive) field, the file will either not be attached because there is no Drive object with that ID, or a different file will be attached.

```json
{
    "result": {
        "ID": 6687,
        "NAME": "ava555.jpg",
        "TYPE": "file",
        "PARENT_ID": "1739",
        "FILE_ID": 28073,
        "SIZE": "405559"
    }
}
```

## 2. Attach the File to the Task

Use the [tasks.task.files.attach](../../api-reference/tasks/tasks-task-files-attach.md) method with the following parameters:

- `taskId` — the task ID. To get the ID, use the [tasks.task.list](../../api-reference/tasks/tasks-task-list.md) method
- `fileId` — specify the file ID `6687` from the result of the previous method

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

- Python

    ```python
    result = client.tasks.task.files.attach(
        task_id=3709,
        file_id=6687,
    ).response.result
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
{% endlist %}

The response returns the link ID between the Drive file and the task: `423`. To verify the attachment using this ID, use the [disk.attachedObject.get](../../api-reference/disk/attached-object/disk-attached-object-get.md) method.

```json
{
    "result": {
        "attachmentId": 423
    }
}
```

## Check the Result

Pass `attachmentId` from the response of the [tasks.task.files.attach](../../api-reference/tasks/tasks-task-files-attach.md) method to the `id` parameter of the [disk.attachedObject.get](../../api-reference/disk/attached-object/disk-attached-object-get.md) method.

{% list tabs %}

- JS

    ```javascript
    const checkResponse = await $b24.actions.v2.call.make({
        method: 'disk.attachedObject.get',
        params: {
            id: result.attachmentId
        },
        requestId: 'disk-attached-object-get'
    })

    if (!checkResponse.isSuccess) {
        throw new Error(checkResponse.getErrorMessages().join('; '))
    }

    console.log(checkResponse.getData().result)
    ```

- Python

    ```python
    file = token.call_method(
        "disk.attachedObject.get",
        {
            "id": result["attachmentId"],
        },
    )["result"]

    print(file)
    ```


- PHP

    ```php
    $file = $serviceBuilder->core->call(
        'disk.attachedObject.get',
        [
            'id' => $result['attachmentId']
        ]
    )->getResponseData()->getResult();

    print_r($file);
    ```
{% endlist %}

The method returns the attached file data. The scenario is successful if:

- `ID` matches `attachmentId` from the previous step
- `OBJECT_ID` contains the Drive file identifier
- `ENTITY_TYPE` equals `tasks_task`
- `ENTITY_ID` equals the task identifier
- `NAME` contains the attached file name

```json
{
    "result": {
        "ID": "423",
        "OBJECT_ID": "6687",
        "MODULE_ID": "tasks",
        "ENTITY_TYPE": "tasks_task",
        "ENTITY_ID": "3709",
        "NAME": "ava555.jpg",
        "SIZE": "405559"
    }
}
```

## Errors and Diagnostics

If the method returns an error, check the request data.

#|
|| **Error** | **Cause and solution** ||
|| `ERROR_NOT_FOUND` in [disk.folder.uploadFile](../../api-reference/disk/folder/disk-folder-upload-file.md) | The folder with the specified `id` was not found ||
|| `DISK_BASE_SERVICE_22001` | The file name was not passed in `data.NAME` ||
|| `ERROR_COULD_NOT_SAVE_FILE` | The file could not be saved. Check free space in Drive and the Base64 data ||
|| `ACCESS_DENIED` | The webhook user does not have permission to add the file to the folder or read the file ||
|| `wrong task id` | An invalid type was passed in `taskId` ||
|| `Could not find value for parameter {fileId}` | The required `fileId` parameter was not passed ||
|| `Invalid value {value} to match with parameter {fileId}` | `fileId` is not a Drive object `ID` ||
|| An empty result in [disk.attachedObject.get](../../api-reference/disk/attached-object/disk-attached-object-get.md) | `attachmentId` from the [tasks.task.files.attach](../../api-reference/tasks/tasks-task-files-attach.md) response was not passed ||
|#

Repeat the scenario from the step that returned the error. If the file has already been uploaded to Drive, do not upload it again: fix `taskId` or `fileId` and repeat only the [tasks.task.files.attach](../../api-reference/tasks/tasks-task-files-attach.md) call.

## Continue Learning

- [How to Create a Task with an Attached File](./how-to-create-task-with-file.md)
- [Upload a File to a Drive Folder disk.folder.uploadFile](../../api-reference/disk/folder/disk-folder-upload-file.md)
- [Attach a File to a Task tasks.task.files.attach](../../api-reference/tasks/tasks-task-files-attach.md)
- [Get an Attached Object disk.attachedObject.get](../../api-reference/disk/attached-object/disk-attached-object-get.md)
