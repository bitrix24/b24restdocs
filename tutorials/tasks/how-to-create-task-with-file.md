# How to Create a Task with an Attached File

> Scope: [`disk`](../../api-reference/scopes/permissions.md), [`task`](../../api-reference/scopes/permissions.md)
>
> Who can execute the methods: to complete the whole scenario, you need permission to add a file to a Drive folder and create a task
>
> - [disk.folder.uploadFile](../../api-reference/disk/folder/disk-folder-upload-file.md) — a user with the Add permission for the Drive folder
> - [tasks.task.add](../../api-reference/tasks/tasks-task-add.md) — any user
> - [tasks.task.get](../../api-reference/tasks/tasks-task-get.md) — a user with access to the task
> - [disk.attachedObject.get](../../api-reference/disk/attached-object/disk-attached-object-get.md) — a user with the Read permission for the file

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Bitrix24 has two types of file fields:

- **File.** This field is not linked to Drive. Files are uploaded directly through a [Base64 format string](../../api-reference/files/how-to-upload-files.md)
- **File (Drive).** This field is linked to Drive. The field stores the Drive object ID. Base64 format is not processed in this field, so the file must first be uploaded to Bitrix24 Drive

To create a task with a file, perform these two methods in sequence:

1. [disk.folder.uploadFile](../../api-reference/disk/folder/disk-folder-upload-file.md) — uploads a file to Drive
2. [tasks.task.add](../../api-reference/tasks/tasks-task-add.md) — creates a task

## Before You Start

You need the following to run the example:

- an inbound webhook with the `disk` and `task` scopes
- the Drive folder identifier `folderId` where the file should be uploaded. You can get a folder using [disk.storage.getchildren](../../api-reference/disk/storage/disk-storage-get-children.md) or [disk.folder.getchildren](../../api-reference/disk/folder/disk-folder-get-children.md)
- the task assignee identifier `RESPONSIBLE_ID`
- the file to attach to the task
- the file name with an extension, for example `ava555.jpg`
- the file content in Base64 format without the `data:*/*;base64,` prefix

The webhook executes requests with the permissions of the user who created it. Do not publish the webhook secret in client-side code or repositories. Store it in environment variables.

For server-side JS examples with `B24Hook`, use Node.js 18, 20, 22, or later. For new projects, use 22 or later. B24JsSDK is an ES module: save the code in a `.mjs` file or add `"type": "module"` to `package.json`.

For b24pysdk examples, use Python 3.9 or later.

The Go examples assume that `ctx` and `core` are already created, the file has been read into the `content` variable, `folderID` and `userID` are known, and `base64`, `encoding/json`, `fmt`, `strconv`, and `github.com/bitrix24/b24gosdk` are imported.

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

- Go

    ```go
    // fileContent is the Bitrix24 file transport: an array with two elements,
    // [file name, Base64 content]. The request body is already JSON, so a regular
    // []string is serialized exactly as the method expects: no multipart or manual
    // URL encoding is required. Base64 increases the data size by about one third,
    // so use this path for small files.
    res, err := core.Call(ctx, "disk.folder.uploadFile", b24.Params{
        "id":          folderID,
        "data":        b24.Params{"NAME": "report.txt"},
        "fileContent": []string{"report.txt", base64.StdEncoding.EncodeToString(content)},
        // Rerunning the example should not fail because of matching names.
        "generateUniqueName": true,
    })
    if err != nil {
        return fmt.Errorf("disk.folder.uploadFile: %w", err)
    }

    var file struct {
        // ID is the Drive object identifier accepted by fields of the File (Drive) type.
        ID b24.ID `json:"ID"`
        // FILE_ID is the internal file identifier. If it is passed to the task field,
        // the file will either not be attached or a different file will be attached.
        FileID b24.ID `json:"FILE_ID"`
        Name   string `json:"NAME"`
    }
    if err := json.Unmarshal(res.Result, &file); err != nil {
        return fmt.Errorf("decode uploaded file: %w", err)
    }
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

## 2. Create the Task with the File

Use the [tasks.task.add](../../api-reference/tasks/tasks-task-add.md) method with the following parameters:

- `UF_TASK_WEBDAV_FILES` — specify the value `n6687`. This is the file ID from the result of the previous method with the `n` prefix added for uploading the file into the field
- `TITLE` — the task title, a required field. The task will not be created without a title
- `RESPONSIBLE_ID` — the assignee ID, a required field. The task will not be created without an assignee

{% list tabs %}

- JS

    ```javascript
    const response = await $b24.actions.v2.call.make({
        method: 'tasks.task.add',
        params: {
            fields: {
                TITLE: 'task for test',
                RESPONSIBLE_ID: 1,
                UF_TASK_WEBDAV_FILES: [
                    'n6687'
                ]
            }
        },
        requestId: 'task-add'
    })

    if (!response.isSuccess) {
        throw new Error(response.getErrorMessages().join('; '))
    }

    const result = response.getData().result
    ```

- Python

    ```python
    result = client.tasks.task.add(
        fields={
            "TITLE": "task for test",
            "RESPONSIBLE_ID": 1,
            "UF_TASK_WEBDAV_FILES": [
                "n6687",
            ],
        }
    ).response.result
    ```

- PHP

    ```php
    $result = $serviceBuilder->core->call(
        'tasks.task.add',
        [
            'fields' => [
                'TITLE' => 'task for test',
                'RESPONSIBLE_ID' => 1,
                'UF_TASK_WEBDAV_FILES' => [
                    'n6687'
                ]
            ]
        ]
    )->getResponseData()->getResult();

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // The "n" prefix before the Drive object identifier means "attach this existing
    // object". The method will not accept a bare number. The field is always an
    // array, even when there is only one file.
    res, err = core.Call(ctx, "tasks.task.add", b24.Params{
        "fields": b24.Params{
            "TITLE":                "Task with file (b24gosdk)",
            "RESPONSIBLE_ID":       userID,
            "UF_TASK_WEBDAV_FILES": []string{"n" + strconv.FormatInt(int64(file.ID), 10)},
        },
    })
    if err != nil {
        return fmt.Errorf("tasks.task.add: %w", err)
    }

    // tasks.* wraps the response in a task object, unlike crm.*.add, which returns
    // a bare identifier. The identifier is returned as a string ("3711"): b24.ID
    // handles both formats, while a regular int does not.
    var out struct {
        Task struct {
            ID    b24.ID `json:"id"`
            Title string `json:"title"`
        } `json:"task"`
    }
    if err := json.Unmarshal(res.Result, &out); err != nil {
        return fmt.Errorf("decode created task: %w", err)
    }
    ```

{% endlist %}

The task is created with ID `3711`.

```json
{
    "result": {
        "task": {
            "id": "3711",
            "title": "task for test",
            "responsibleId": "1",
            "status": "2"
        }
    }
}
```

The response does not contain task file details. To verify that the file was attached successfully, call the [tasks.task.get](../../api-reference/tasks/tasks-task-get.md) method with the `UF_TASK_WEBDAV_FILES` field in `SELECT`.

The [tasks.task.get](../../api-reference/tasks/tasks-task-get.md) method returns the ID of the record that represents the link between the Drive file and the task. To get file data by that link ID, use the [disk.attachedObject.get](../../api-reference/disk/attached-object/disk-attached-object-get.md) method.

## Check the Result

{% list tabs %}

- JS

    ```javascript
    const checkResponse = await $b24.actions.v2.call.make({
        method: 'tasks.task.get',
        params: {
            taskId: 3711,
            select: ['ID', 'TITLE', 'UF_TASK_WEBDAV_FILES']
        },
        requestId: 'task-get-check'
    })

    if (!checkResponse.isSuccess) {
        throw new Error(checkResponse.getErrorMessages().join('; '))
    }

    const task = checkResponse.getData().result.task
    const attachmentId = task.ufTaskWebdavFiles[0]

    const fileResponse = await $b24.actions.v2.call.make({
        method: 'disk.attachedObject.get',
        params: {
            id: attachmentId
        },
        requestId: 'disk-attached-object-get'
    })

    if (!fileResponse.isSuccess) {
        throw new Error(fileResponse.getErrorMessages().join('; '))
    }

    console.log(fileResponse.getData().result)
    ```

- Python

    ```python
    task = client.tasks.task.get(
        bitrix_id=3711,
        select=["ID", "TITLE", "UF_TASK_WEBDAV_FILES"],
    ).response.result["task"]

    attachment_id = task["ufTaskWebdavFiles"][0]

    file = token.call_method(
        "disk.attachedObject.get",
        {
            "id": attachment_id,
        },
    )["result"]

    print(file)
    ```

- PHP

    ```php
    $task = $serviceBuilder->core->call(
        'tasks.task.get',
        [
            'taskId' => 3711,
            'select' => ['ID', 'TITLE', 'UF_TASK_WEBDAV_FILES']
        ]
    )->getResponseData()->getResult()['task'];

    $attachmentId = $task['ufTaskWebdavFiles'][0];

    $file = $serviceBuilder->core->call(
        'disk.attachedObject.get',
        [
            'id' => $attachmentId
        ]
    )->getResponseData()->getResult();

    print_r($file);
    ```

- Go

    ```go
    res, err = core.Call(ctx, "tasks.task.get", b24.Params{
        "taskId": out.Task.ID,
        "select": []string{"ID", "TITLE", "UF_TASK_WEBDAV_FILES"},
    })
    if err != nil {
        return fmt.Errorf("tasks.task.get: %w", err)
    }

    var taskCheck struct {
        Task struct {
            ID                b24.ID   `json:"id"`
            Title             string   `json:"title"`
            UfTaskWebdavFiles []b24.ID `json:"ufTaskWebdavFiles"`
        } `json:"task"`
    }
    if err := json.Unmarshal(res.Result, &taskCheck); err != nil {
        return fmt.Errorf("decode task: %w", err)
    }
    if len(taskCheck.Task.UfTaskWebdavFiles) == 0 {
        return fmt.Errorf("the task has no attached files")
    }

    res, err = core.Call(ctx, "disk.attachedObject.get", b24.Params{
        "id": taskCheck.Task.UfTaskWebdavFiles[0],
    })
    if err != nil {
        return fmt.Errorf("disk.attachedObject.get: %w", err)
    }

    var attachment struct {
        ID         b24.ID `json:"ID"`
        ObjectID   b24.ID `json:"OBJECT_ID"`
        EntityType string `json:"ENTITY_TYPE"`
        EntityID   b24.ID `json:"ENTITY_ID"`
        Name       string `json:"NAME"`
    }
    if err := json.Unmarshal(res.Result, &attachment); err != nil {
        return fmt.Errorf("decode attached file: %w", err)
    }
    ```

{% endlist %}

The `ufTaskWebdavFiles` field contains the identifiers of links between the task and Drive files. This is not the file `ID` itself, but the attachment `ID`.

```json
{
    "result": {
        "task": {
            "id": "3711",
            "title": "task for test",
            "ufTaskWebdavFiles": [
                423
            ]
        }
    }
}
```

To get file data, pass the value `423` to the `id` parameter of the [disk.attachedObject.get](../../api-reference/disk/attached-object/disk-attached-object-get.md) method. The scenario is successful if these response fields are returned:

- `ID` — the file attachment identifier
- `OBJECT_ID` — the file identifier in Drive
- `ENTITY_TYPE` — the object type to which the file is attached. For a task, this value is `tasks_task`
- `ENTITY_ID` — the task identifier
- `NAME` — the attached file name

## Errors and Diagnostics

If the method returns an error, check the request data.

#|
|| **Error** | **Cause and solution** ||
|| `ERROR_NOT_FOUND` in [disk.folder.uploadFile](../../api-reference/disk/folder/disk-folder-upload-file.md) | The folder with the specified `id` was not found ||
|| `DISK_BASE_SERVICE_22001` | The file name was not passed in `data.NAME` ||
|| `ERROR_COULD_NOT_SAVE_FILE` | The file could not be saved. Check free space in Drive and the Base64 data ||
|| `ACCESS_DENIED` | The webhook user does not have permission to add the file to the folder ||
|| `ERROR_CORE` in [tasks.task.add](../../api-reference/tasks/tasks-task-add.md) | Check `TITLE`, `RESPONSIBLE_ID`, and required custom task fields ||
|| An empty `UF_TASK_WEBDAV_FILES` value during verification | `FILE_ID` was passed instead of the Drive object `ID`, or the `n` prefix was not added ||
|#

Repeat the scenario from the step that returned the error. If the file has already been uploaded to Drive, do not upload it again: fix the task parameters and repeat only the [tasks.task.add](../../api-reference/tasks/tasks-task-add.md) call.

## What to Consider

- In the `UF_TASK_WEBDAV_FILES` field, pass the Drive object `ID` from the [disk.folder.uploadFile](../../api-reference/disk/folder/disk-folder-upload-file.md) response, not `FILE_ID`
- When creating the task, add the `n` prefix to the Drive object `ID`, for example `n6687`
- The `UF_TASK_WEBDAV_FILES` field is always passed as an array, even if there is only one file
- Re-running the example creates a new task and can upload a new file with the same name if unique names are enabled in the upload request

## Continue Learning

- [How to Upload a File to a Task](./how-to-upload-file-to-task.md)
- [Upload a File to a Drive Folder disk.folder.uploadFile](../../api-reference/disk/folder/disk-folder-upload-file.md)
- [Get Folder Contents disk.folder.getchildren](../../api-reference/disk/folder/disk-folder-get-children.md)
- [Create a Task tasks.task.add](../../api-reference/tasks/tasks-task-add.md)
- [Get an Attached Object disk.attachedObject.get](../../api-reference/disk/attached-object/disk-attached-object-get.md)
