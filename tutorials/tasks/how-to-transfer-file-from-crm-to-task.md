# How to Transfer a File from a CRM Field to a Task

> Scope: [`crm`](../../api-reference/scopes/permissions.md), [`disk`](../../api-reference/scopes/permissions.md), [`task`](../../api-reference/scopes/permissions.md)
>
> Who can execute the methods: to complete the entire scenario, you need permission to read CRM items, add files to a Drive folder, and edit the task
>
> - [crm.item.get](../../api-reference/crm/universal/crm-item-get.md) — a user with permission to read items of a CRM object
> - [disk.folder.uploadFile](../../api-reference/disk/folder/disk-folder-upload-file.md) — a user with the Add permission for the Drive folder
> - [tasks.task.files.attach](../../api-reference/tasks/tasks-task-files-attach.md) — the task creator or a user with permission to edit the task and read the file

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

A manager attached a contract to a deal, and the assignee will work with that file in a task. A single call cannot copy the file between the two objects: deals and tasks store files differently.

A deal file field is a field of the File type: it holds a CRM file ID. Tasks work differently. A task stores its attachments in the `UF_TASK_WEBDAV_FILES` field of the File (Drive) type, and that field accepts only a Drive object ID.

These are two separate sets of identifiers. A CRM file ID means nothing to Drive: Drive methods will return an error for it, or a different file that happens to share the same number.

CRM has no File (Drive) fields, so there is no ready-made Drive object ID to take from a deal. The file has to be transferred manually: download it from the deal, upload it to Drive, and attach it to the task from there.

The scenario consists of four steps.

1. Get the file link using the [crm.item.get](../../api-reference/crm/universal/crm-item-get.md) method
2. Download the file from that link with a regular HTTP request
3. Upload the file to Drive using the [disk.folder.uploadFile](../../api-reference/disk/folder/disk-folder-upload-file.md) method
4. Attach the file to the task using the [tasks.task.files.attach](../../api-reference/tasks/tasks-task-files-attach.md) method

The scenario is shown for a single file.

## Before You Start

Prepare the scenario data:

- **A CRM item with a filled file field.** You will need the `entityTypeId` of the object, the `id` of the item, and the field name, such as `ufCrm_1688736288`. The examples use a deal, whose `entityTypeId` is `2`. The [crm.item.fields](../../api-reference/crm/universal/crm-item-fields.md) method returns the list of fields, and file fields have the `file` type
- **A task to attach the file to.** You will need the task `ID`, which the [tasks.task.list](../../api-reference/tasks/tasks-task-list.md) method returns
- **A Drive folder for the upload.** You will need the folder `ID`. The [disk.storage.getlist](../../api-reference/disk/storage/disk-storage-get-list.md) and [disk.storage.getchildren](../../api-reference/disk/storage/disk-storage-get-children.md) methods return storages and their contents
- **REST access.** A webhook or an application with the `crm`, `disk`, and `task` permissions

{% include [Note on examples](../../_includes/examples.md) %}

## 1. Get the File Link from the CRM Item

Read the deal with the [crm.item.get](../../api-reference/crm/universal/crm-item-get.md) method and the following parameters:

- `entityTypeId` — specify `2`, the identifier of the deal type
- `id` — specify `6533`, the identifier of the deal that holds the file

{% list tabs %}

- JS

    ```javascript
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    const response = await $b24.actions.v2.call.make({
        method: 'crm.item.get',
        params: {
            entityTypeId: 2,
            id: 6533
        },
        requestId: 'crm-item-get'
    })

    if (!response.isSuccess) {
        throw new Error(response.getErrorMessages().join('; '))
    }

    const item = response.getData().result.item
    const file = item.ufCrm_1688736288
    ```

- Python

    ```python
    from b24pysdk import BitrixWebhook, Client

    token = BitrixWebhook(
        domain="your-domain.bitrix24.com",
        webhook_token="user_id/webhook_key",
    )
    client = Client(token)

    # Called through the SDK core: the typed wrapper does not return custom fields
    item = token.call_method("crm.item.get", {
        "entityTypeId": 2,
        "id": 6533,
    })["result"]["item"]

    file = item["ufCrm_1688736288"]
    ```

- PHP

    ```php
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Symfony\Component\EventDispatcher\EventDispatcher;

    $serviceBuilder = (new ServiceBuilderFactory(new EventDispatcher(), $log))
        ->initFromWebhook('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/');

    // Called through the SDK core: the typed wrapper does not return custom fields
    $item = $serviceBuilder->core->call(
        'crm.item.get',
        ['entityTypeId' => 2, 'id' => 6533]
    )->getResponseData()->getResult()['item'];

    $file = $item['ufCrm_1688736288'];
    ```
{% endlist %}

The file field returns three values:

- `id` — the CRM file identifier. Drive has no object with this number, so it must not be passed to Drive methods
- `url` — a link for the Bitrix24 interface. It opens in the browser of a signed-in user and is not suitable for an integration
- `urlMachine` — a link for integrations. It carries an access token and a file signature, so the file can be downloaded with a regular HTTP request. This is the link used in the next step

```json
{
    "result": {
        "item": {
            "id": 6533,
            "title": "Equipment supply",
            "ufCrm_1688736288": {
                "id": 27807,
                "url": "https://your-domain.bitrix24.com/bitrix/services/main/ajax.php?action=crm.controller.item.getFile&SITE_ID=s1&entityTypeId=2&id=6533&fieldName=UF_CRM_1688736288&fileId=27807",
                "urlMachine": "https://your-domain.bitrix24.com/rest/1/xxxxxxxxxxxxxxxx/crm.controller.item.getFile/?token=xxxxxxxx"
            }
        }
    }
}
```

If the field is multiple, an array of such objects arrives instead of a single object, and steps 2, 3, and 4 are repeated for each file.

## 2. Download the File

This step calls no method: the file is retrieved with a regular HTTP request to the `urlMachine` link.

The request needs no separate authorization, because the token is already inside the link. Pass the link as a whole and do not split it into parts, since the set of parameters may change. The link is usually absolute, but if it starts with `/` in the response, prepend your Bitrix24 address to it.

Next, the file has to be encoded in Base64, because Drive accepts it in that form. Encoding is covered in the [How to Upload Files](../../api-reference/files/how-to-upload-files.md) article.

{% list tabs %}

- JS

    ```javascript
    const fileResponse = await fetch(file.urlMachine)

    if (!fileResponse.ok) {
        throw new Error('Failed to download the file: ' + fileResponse.status)
    }

    const buffer = Buffer.from(await fileResponse.arrayBuffer())
    const fileContent = buffer.toString('base64')
    ```

- Python

    ```python
    import base64

    import requests

    file_response = requests.get(file["urlMachine"], timeout=30)
    file_response.raise_for_status()

    file_content = base64.b64encode(file_response.content).decode()
    ```

- PHP

    ```php
    $binary = file_get_contents($file['urlMachine']);

    if ($binary === false) {
        throw new RuntimeException('Failed to download the file');
    }

    $fileContent = base64_encode($binary);
    ```
{% endlist %}

The original file name is returned in the `Content-Disposition` header. In the examples below the name is set manually, so the file will be named as specified in `data.NAME` on Drive and in the task, not as it was named in the deal. To keep the original name, take it from the header and pass it in the next step.

## 3. Upload the File to Drive

Upload the file with the [disk.folder.uploadFile](../../api-reference/disk/folder/disk-folder-upload-file.md) method and the following parameters:

- `id` — specify `1739`, the identifier of the Drive folder
- `data` — pass the name in `NAME`. The file is saved on Drive with this name
- `fileContent` — pass an array of the file name and the Base64 string from the previous step

{% list tabs %}

- JS

    ```javascript
    const uploadResponse = await $b24.actions.v2.call.make({
        method: 'disk.folder.uploadFile',
        params: {
            id: 1739,
            data: {
                NAME: 'contract.pdf'
            },
            fileContent: [
                'contract.pdf',
                fileContent
            ]
        },
        requestId: 'disk-uploadfile'
    })

    if (!uploadResponse.isSuccess) {
        throw new Error(uploadResponse.getErrorMessages().join('; '))
    }

    const diskFile = uploadResponse.getData().result
    ```

- Python

    ```python
    disk_file = client.disk.folder.uploadfile(
        bitrix_id=1739,
        data={
            "NAME": "contract.pdf",
        },
        file_content=[
            "contract.pdf",
            file_content,
        ],
    ).response.result
    ```

- PHP

    ```php
    $diskFile = $serviceBuilder->getDiskScope()->folder()->uploadFile(
        1739,
        ['NAME' => 'contract.pdf'],
        ['contract.pdf', $fileContent]
    )->getFile();
    ```
{% endlist %}

The response returns two different identifiers, and it is important not to mix them up:

- `ID` — the Drive object identifier. This is the value passed to the task
- `FILE_ID` — the internal file identifier. It must not be passed to fields of the File (Drive) type

```json
{
    "result": {
        "ID": 6687,
        "NAME": "contract.pdf",
        "TYPE": "file",
        "PARENT_ID": "1739",
        "FILE_ID": 28073,
        "SIZE": "405559"
    }
}
```

## 4. Attach the File to the Task

Attach the file with the [tasks.task.files.attach](../../api-reference/tasks/tasks-task-files-attach.md) method and the following parameters:

- `taskId` — specify `3709`, the identifier of the task
- `fileId` — specify `6687`, the `ID` value from the previous response

{% list tabs %}

- JS

    ```javascript
    const attachResponse = await $b24.actions.v2.call.make({
        method: 'tasks.task.files.attach',
        params: {
            taskId: 3709,
            fileId: diskFile.ID
        },
        requestId: 'task-files-attach'
    })

    if (!attachResponse.isSuccess) {
        throw new Error(attachResponse.getErrorMessages().join('; '))
    }

    const attachment = attachResponse.getData().result
    ```

- Python

    ```python
    attachment = client.tasks.task.files.attach(
        task_id=3709,
        file_id=disk_file["ID"],
    ).response.result
    ```

- PHP

    ```php
    $attachment = $serviceBuilder->core->call(
        'tasks.task.files.attach',
        [
            'taskId' => 3709,
            'fileId' => $diskFile['ID']
        ]
    )->getResponseData()->getResult();
    ```
{% endlist %}

The method returns `attachmentId`, the identifier of the link between the Drive file and the task.

```json
{
    "result": {
        "attachmentId": 423
    }
}
```

## Check the Result

The file should appear in the task. There are two ways to verify this.

In the Bitrix24 interface, open the task — the file will be in the attachments block.

Through REST, pass `attachmentId` to the `id` parameter of the [disk.attachedObject.get](../../api-reference/disk/attached-object/disk-attached-object-get.md) method.

{% list tabs %}

- JS

    ```javascript
    const checkResponse = await $b24.actions.v2.call.make({
        method: 'disk.attachedObject.get',
        params: {
            id: attachment.attachmentId
        },
        requestId: 'disk-attached-object-get'
    })

    console.log(checkResponse.getData().result)
    ```

- Python

    ```python
    check = token.call_method(
        "disk.attachedObject.get",
        {
            "id": attachment["attachmentId"],
        },
    )["result"]

    print(check)
    ```

- PHP

    ```php
    $check = $serviceBuilder->core->call(
        'disk.attachedObject.get',
        ['id' => $attachment['attachmentId']]
    )->getResponseData()->getResult();

    print_r($check);
    ```
{% endlist %}

The scenario is successful if:

- `ENTITY_TYPE` equals `tasks_task`
- `ENTITY_ID` equals the task identifier
- `OBJECT_ID` equals the `ID` of the file on Drive from the third step
- `NAME` contains the name under which the file was saved on Drive

```json
{
    "result": {
        "ID": "423",
        "OBJECT_ID": "6687",
        "MODULE_ID": "tasks",
        "ENTITY_TYPE": "tasks_task",
        "ENTITY_ID": "3709",
        "NAME": "contract.pdf",
        "SIZE": "405559"
    }
}
```

## Errors and Diagnostics

If the method returns an error, check the request data.

#|
|| **Code** | **Cause and action** ||
|| `NOT_FOUND` in [crm.item.get](../../api-reference/crm/universal/crm-item-get.md) | No item with this `id` exists, or the user has no permission to read it ||
|| `ENTITY_TYPE_NOT_SUPPORTED` | The `entityTypeId` is invalid. Check it against the [CRM object types reference](../../api-reference/crm/data-types.md#object_type) ||
|| The file field returns `null` | The field is not filled for this item. Take an item that has a file ||
|| The file field is missing from the response | The field name is incorrect. Check the name with the [crm.item.fields](../../api-reference/crm/universal/crm-item-fields.md) method ||
|| An HTML sign-in page is downloaded instead of the file | The `url` link was used instead of `urlMachine` ||
|| `ERROR_NOT_FOUND` in [disk.folder.uploadFile](../../api-reference/disk/folder/disk-folder-upload-file.md) | The folder with the specified `id` was not found, or the user has no access to it ||
|| `DISK_BASE_SERVICE_22001` | The file name was not passed in `data.NAME` ||
|| `DISK_OBJ_22000` | The folder already contains a file with this name. Use a different name or pass `generateUniqueName` with the value `true` ||
|| `DISK_FOLDER_22002` | The file could not be saved. Check free space in Drive and the Base64 data ||
|| `ERROR_CORE` with the text about a file that could not be found in [tasks.task.files.attach](../../api-reference/tasks/tasks-task-files-attach.md) | `fileId` is not a Drive object `ID`. A common cause is passing `FILE_ID` or the CRM file `id` ||
|| `ERROR_NOT_FOUND` in [disk.attachedObject.get](../../api-reference/disk/attached-object/disk-attached-object-get.md) | The value passed is not the `attachmentId` from the [tasks.task.files.attach](../../api-reference/tasks/tasks-task-files-attach.md) response ||
|#

If there was no error but the file is not in the task, check the following:

- `taskId` points to an existing task. With an invalid identifier, the [tasks.task.files.attach](../../api-reference/tasks/tasks-task-files-attach.md) method still returns `attachmentId`, but the file does not appear in the task
- `fileId` holds the Drive object `ID` from the [disk.folder.uploadFile](../../api-reference/disk/folder/disk-folder-upload-file.md) response, not `FILE_ID`
- the file was uploaded to a folder the user has access to

Repeat the scenario from the step that returned the error. If the file is already uploaded to Drive, do not upload it again: correct `taskId` or `fileId` and repeat only the [tasks.task.files.attach](../../api-reference/tasks/tasks-task-files-attach.md) call.

## Key Considerations

- A file in a CRM field is not a Drive object. It cannot be passed to a task directly or as a Base64 string: the `UF_TASK_WEBDAV_FILES` field accepts only a Drive object identifier
- If you attach the file by writing to the `UF_TASK_WEBDAV_FILES` field instead of calling [tasks.task.files.attach](../../api-reference/tasks/tasks-task-files-attach.md) — for example when creating a task with the [tasks.task.add](../../api-reference/tasks/tasks-task-add.md) method — pass the value in the `n<identifier>` format, such as `["n6687"]`. The field does not accept a number without the prefix
- The `urlMachine` link carries an access token and a file signature. Do not write it to logs, do not store it in a database, and do not pass it to third parties
- The scenario works with objects other than deals. For another CRM object, change `entityTypeId` and take the file field name from [crm.item.fields](../../api-reference/crm/universal/crm-item-fields.md). For smart processes, the field name includes the process number, such as `ufCrm_7_1739432938`
- The file is copied, not moved. It remains in the deal, and a separate object appears on Drive. If the original file in the deal is later replaced, the copy in the task is not updated

## Code Example

The code goes through all four steps: it reads the CRM item, downloads the file, uploads it to Drive, and attaches it to the task.

Replace the webhook, the identifiers of the item, folder, and task, and the file field name.

{% list tabs %}

- JS

    ```javascript
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)

    const entityTypeId = 2
    const itemId = 6533
    const fieldName = 'ufCrm_1688736288'
    const folderId = 1739
    const taskId = 3709
    const fileName = 'contract.pdf'

    async function call(method, params, requestId) {
        const response = await $b24.actions.v2.call.make({ method, params, requestId })

        if (!response.isSuccess) {
            throw new Error(method + ': ' + response.getErrorMessages().join('; '))
        }

        return response.getData().result
    }

    // 1. Read the CRM item and take the link for integrations
    const { item } = await call('crm.item.get', { entityTypeId, id: itemId }, 'crm-item-get')
    const file = item[fieldName]

    if (!file || !file.urlMachine) {
        throw new Error('Field ' + fieldName + ' has no file or urlMachine link')
    }

    // 2. Download the file and encode it in Base64
    const fileResponse = await fetch(file.urlMachine)

    if (!fileResponse.ok) {
        throw new Error('Failed to download the file: ' + fileResponse.status)
    }

    const fileContent = Buffer.from(await fileResponse.arrayBuffer()).toString('base64')

    // 3. Upload the file to Drive
    const diskFile = await call('disk.folder.uploadFile', {
        id: folderId,
        data: { NAME: fileName },
        fileContent: [fileName, fileContent]
    }, 'disk-uploadfile')

    // 4. Attach the Drive file to the task
    const attachment = await call('tasks.task.files.attach', {
        taskId: taskId,
        fileId: diskFile.ID
    }, 'task-files-attach')

    console.log('File attached, attachmentId:', attachment.attachmentId)
    ```

- Python

    ```python
    import base64

    import requests
    from b24pysdk import BitrixWebhook, Client

    token = BitrixWebhook(
        domain="your-domain.bitrix24.com",
        webhook_token="user_id/webhook_key",
    )
    client = Client(token)

    entity_type_id = 2
    item_id = 6533
    field_name = "ufCrm_1688736288"
    folder_id = 1739
    task_id = 3709
    file_name = "contract.pdf"

    # 1. Read the CRM item and take the link for integrations
    item = token.call_method("crm.item.get", {
        "entityTypeId": entity_type_id,
        "id": item_id,
    })["result"]["item"]

    file = item.get(field_name)

    if not file or not file.get("urlMachine"):
        raise ValueError(f"Field {field_name} has no file or urlMachine link")

    # 2. Download the file and encode it in Base64
    file_response = requests.get(file["urlMachine"], timeout=30)
    file_response.raise_for_status()
    file_content = base64.b64encode(file_response.content).decode()

    # 3. Upload the file to Drive
    disk_file = client.disk.folder.uploadfile(
        bitrix_id=folder_id,
        data={"NAME": file_name},
        file_content=[file_name, file_content],
    ).response.result

    # 4. Attach the Drive file to the task
    attachment = client.tasks.task.files.attach(
        task_id=task_id,
        file_id=disk_file["ID"],
    ).response.result

    print("File attached, attachmentId:", attachment["attachmentId"])
    ```

- PHP

    ```php
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Symfony\Component\EventDispatcher\EventDispatcher;

    $serviceBuilder = (new ServiceBuilderFactory(new EventDispatcher(), $log))
        ->initFromWebhook('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/');

    $entityTypeId = 2;
    $itemId = 6533;
    $fieldName = 'ufCrm_1688736288';
    $folderId = 1739;
    $taskId = 3709;
    $fileName = 'contract.pdf';

    // 1. Read the CRM item and take the link for integrations
    $item = $serviceBuilder->core->call(
        'crm.item.get',
        ['entityTypeId' => $entityTypeId, 'id' => $itemId]
    )->getResponseData()->getResult()['item'];

    $file = $item[$fieldName] ?? null;

    if (!$file || empty($file['urlMachine'])) {
        throw new RuntimeException('Field ' . $fieldName . ' has no file or urlMachine link');
    }

    // 2. Download the file and encode it in Base64
    $binary = file_get_contents($file['urlMachine']);

    if ($binary === false) {
        throw new RuntimeException('Failed to download the file');
    }

    $fileContent = base64_encode($binary);

    // 3. Upload the file to Drive
    $diskFile = $serviceBuilder->getDiskScope()->folder()->uploadFile(
        $folderId,
        ['NAME' => $fileName],
        [$fileName, $fileContent]
    )->getFile();

    // 4. Attach the Drive file to the task
    $attachment = $serviceBuilder->core->call(
        'tasks.task.files.attach',
        ['taskId' => $taskId, 'fileId' => $diskFile['ID']]
    )->getResponseData()->getResult();

    echo 'File attached, attachmentId: ' . $attachment['attachmentId'];
    ```
{% endlist %}

## Continue Learning

- [How to upload a file to a task](./how-to-upload-file-to-task.md)
- [How to create a task with an attached file](./how-to-create-task-with-file.md)
- [How to Download Files](../../api-reference/files/how-to-download-files.md)
- [How to Upload Files](../../api-reference/files/how-to-upload-files.md)
- [Get a CRM item crm.item.get](../../api-reference/crm/universal/crm-item-get.md)
- [Upload a file to a Drive folder disk.folder.uploadFile](../../api-reference/disk/folder/disk-folder-upload-file.md)
- [Attach a file to a task tasks.task.files.attach](../../api-reference/tasks/tasks-task-files-attach.md)
