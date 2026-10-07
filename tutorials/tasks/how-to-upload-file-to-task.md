# How to Upload a File to a Task

> Scope: [`disk`](../../api-reference/scopes/permissions.md), [`task`](../../api-reference/scopes/permissions.md)
>
> Who can execute the methods: to complete the whole scenario, you need permission to add files to a Drive folder, edit the task, and read the file
>
> - [disk.folder.uploadFile](../../api-reference/disk/folder/disk-folder-upload-file.md) — a user with the Add permission for the Drive folder
> - [tasks.task.files.attach](../../api-reference/tasks/tasks-task-files-attach.md) — the task creator or a user with permission to edit the task and read the file
> - [disk.attachedObject.get](../../api-reference/disk/attached-object/disk-attached-object-get.md) — a user with the Read permission for the file

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Bitrix24 has two types of file fields:

- **File.** This field is not linked to Drive. Files are uploaded directly through a [Base64 format string](../../api-reference/files/how-to-upload-files.md)
- **File (Drive).** This field is linked to Drive. The field stores the Drive object ID. Base64 format is not processed in this field, so the file must first be uploaded to Bitrix24 Drive

The scenario has three steps:

1. Upload the file to Drive using [disk.folder.uploadFile](../../api-reference/disk/folder/disk-folder-upload-file.md)
2. Pass the Drive object's `ID` to [tasks.task.files.attach](../../api-reference/tasks/tasks-task-files-attach.md) to attach it to the task
3. Verify the file's link to the task using [disk.attachedObject.get](../../api-reference/disk/attached-object/disk-attached-object-get.md)

The file will appear in an existing task. Verify the attachment after attaching it: the second method returns the `attachmentId` needed for the third step.

## Before You Start

To run the example, you need:

- an inbound webhook with the `disk` and `task` scopes, created by a user with permission to add a file to a Drive folder, edit the task, and read the file
- the ID `folderId` of the Drive folder where you will upload the file. Retrieve it using [disk.storage.getChildren](../../api-reference/disk/storage/disk-storage-get-children.md) for a folder at the storage root or [disk.folder.getChildren](../../api-reference/disk/folder/disk-folder-get-children.md) for a nested folder. The examples use `1739`
- the ID `taskId` of an existing task. Retrieve it using [tasks.task.list](../../api-reference/tasks/tasks-task-list.md). The examples use `3709`
- the file to attach, located in the directory where the script runs. The source file name is `avatar.jpg`; its name on Drive, from `data.NAME`, is `ava555.jpg`
- the file contents as a Base64 string without a `data:*/*;base64,` prefix. Pass `fileContent` as an array containing the file name and this string

The webhook runs with the permissions of the user who created it. Its URL grants access to methods within its scopes: store it in server environment variables, not in browser code or a repository. For JS and PHP, set `B24_HOOK` to the full webhook URL. For Python, set `B24_DOMAIN` to the Bitrix24 domain and `B24_WEBHOOK_TOKEN` to a value in the form `USER_ID/TOKEN`.

The JS example requires Node.js 22 or later and `@bitrix24/b24jssdk`. The code uses ES modules: save it in a `.mjs` file or add `"type": "module"` to `package.json`. The Python example requires Python 3.9 or later and `b24pysdk`. The PHP example requires PHP 8.4 or later and `bitrix24/b24phpsdk` version `^3.0`.

Initialize the SDK before calling a method. Put the initialization code and the following snippets for your chosen language in the same script.

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```javascript
    import { readFile } from 'node:fs/promises'
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    ```

- PHP

    ```php
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Monolog\Logger;
    use Symfony\Component\EventDispatcher\EventDispatcher;

    $log = new Logger('b24');
    $serviceBuilder = (new ServiceBuilderFactory(new EventDispatcher(), $log))
        ->initFromWebhook(getenv('B24_HOOK'));
    ```

- Python

    ```python
    import base64
    import os
    from pathlib import Path

    from b24pysdk import BitrixWebhook, Client

    token = BitrixWebhook(
        domain=os.environ["B24_DOMAIN"],
        webhook_token=os.environ["B24_WEBHOOK_TOKEN"],
    )
    client = Client(token)
    ```

{% endlist %}

## 1. Upload the File to Bitrix24 Drive

Use the [disk.folder.uploadFile](../../api-reference/disk/folder/disk-folder-upload-file.md) method with the following parameters:

- `id` — specify the value `1739`, the identifier of the Drive folder where the file is uploaded
- `data` — specify the file name in `NAME`. The file will be saved in Bitrix24 Drive with this name
- `fileContent` — pass the file in the format `['file_name.extension', 'file as a Base64-encoded string']`

Uploading the file to Drive is required because the `UF_TASK_WEBDAV_FILES` field in tasks accepts only Drive file IDs.

{% list tabs %}

- JS

    ```javascript
    const fileName = 'avatar.jpg'
    const fileBase64 = (await readFile(fileName)).toString('base64')

    const uploadResponse = await $b24.actions.v2.call.make({
        method: 'disk.folder.uploadFile',
        params: {
            id: 1739,
            data: {
                NAME: 'ava555.jpg'
            },
            fileContent: [
                fileName,
                fileBase64
            ]
        },
        requestId: 'disk-uploadfile'
    })

    if (!uploadResponse.isSuccess) {
        throw new Error(uploadResponse.getErrorMessages().join('; '))
    }

    const uploadedFile = uploadResponse.getData().result
    ```

- PHP

    ```php
    $fileName = 'avatar.jpg';
    $fileBytes = file_get_contents($fileName);
    if ($fileBytes === false) {
        throw new RuntimeException('Could not read the file');
    }
    $fileBase64 = base64_encode($fileBytes);

    $uploadedFile = $serviceBuilder->getDiskScope()->folder()->uploadFile(
        1739,
        ['NAME' => 'ava555.jpg'],
        [
            $fileName,
            $fileBase64
        ]
    )->getFile();

    echo '<PRE>';
    print_r($uploadedFile);
    echo '</PRE>';
    ```

- Python

    ```python
    file_name = "avatar.jpg"
    file_base64 = base64.b64encode(Path(file_name).read_bytes()).decode("ascii")

    uploaded_file = client.disk.folder.uploadfile(
        bitrix_id=1739,
        data={
            "NAME": "ava555.jpg",
        },
        file_content=[
            file_name,
            file_base64,
        ],
    ).response.result
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
- `fileId` — pass the Drive object's `ID` from the previous method's response. In the example response, this is `6687`

{% list tabs %}

- JS

    ```javascript
    const attachResponse = await $b24.actions.v2.call.make({
        method: 'tasks.task.files.attach',
        params: {
            taskId: 3709,
            fileId: Number(uploadedFile.ID)
        },
        requestId: 'task-files-attach'
    })

    if (!attachResponse.isSuccess) {
        throw new Error(attachResponse.getErrorMessages().join('; '))
    }

    const attachment = attachResponse.getData().result
    ```

- PHP

    ```php
    // This method has no typed wrapper, so call it through the SDK core
    $attachment = $serviceBuilder->core->call(
        'tasks.task.files.attach',
        [
            'taskId' => 3709,
            'fileId' => $uploadedFile['ID']
        ]
    )->getResponseData()->getResult();

    echo '<PRE>';
    print_r($attachment);
    echo '</PRE>';
    ```

- Python

    ```python
    attachment = client.tasks.task.files.attach(
        task_id=3709,
        file_id=int(uploaded_file["ID"]),
    ).response.result
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

Pass `attachmentId` from the [tasks.task.files.attach](../../api-reference/tasks/tasks-task-files-attach.md) response to the `id` parameter of [disk.attachedObject.get](../../api-reference/disk/attached-object/disk-attached-object-get.md). The example response contains `423`; the code uses the ID of the current attachment.

{% list tabs %}

- JS

    ```javascript
    const checkResponse = await $b24.actions.v2.call.make({
        method: 'disk.attachedObject.get',
        params: {
            id: Number(attachment.attachmentId)
        },
        requestId: 'disk-attached-object-get'
    })

    if (!checkResponse.isSuccess) {
        throw new Error(checkResponse.getErrorMessages().join('; '))
    }

    console.log(checkResponse.getData().result)
    ```

- PHP

    ```php
    // This method has no typed wrapper, so call it through the SDK core
    $file = $serviceBuilder->core->call(
        'disk.attachedObject.get',
        [
            'id' => $attachment['attachmentId']
        ]
    )->getResponseData()->getResult();

    print_r($file);
    ```

- Python

    ```python
    # This method has no typed wrapper, so use a direct call
    file = token.call_method(
        "disk.attachedObject.get",
        {
            "id": attachment["attachmentId"],
        },
    )["result"]

    print(file)
    ```
{% endlist %}

The method returns the attached file data. The scenario is successful if:

- `ID` matches `attachmentId` from the previous step
- `OBJECT_ID` contains the Drive file identifier
- `ENTITY_TYPE` equals `tasks_task`
- `ENTITY_ID` equals the task identifier
- `NAME` contains the attached file name

Open the task in Bitrix24 and check that `ava555.jpg` appears among the attached files.

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

## Things to Consider

If the file is already on Drive, skip the upload and pass its `ID` to `fileId` in [tasks.task.files.attach](../../api-reference/tasks/tasks-task-files-attach.md). To use a different task, change `taskId`. In both cases, check that the webhook user has access to the file and task.

## Continue Learning

- [How to Create a Task with an Attached File](./how-to-create-task-with-file.md)
- [Upload a File to a Drive Folder disk.folder.uploadFile](../../api-reference/disk/folder/disk-folder-upload-file.md)
- [Attach a File to a Task tasks.task.files.attach](../../api-reference/tasks/tasks-task-files-attach.md)
- [Get an Attached Object disk.attachedObject.get](../../api-reference/disk/attached-object/disk-attached-object-get.md)
