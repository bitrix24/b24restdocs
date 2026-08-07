# How to Create a Task with an Attached File

> Scope: [`disk`, `tasks`](../../api-reference/scopes/permissions.md)
>
> Who can execute the method: users with access to the Drive and Task sections

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../ai-tools/mcp.md) so that the assistant uses the official REST documentation.

{% endnote %}

Bitrix24 has two types of file fields: 

* **File.** This field is not linked to Drive; files are uploaded directly via a [Base64 format string](../../api-reference/files/how-to-upload-files.md)
* **File (Drive).** This field is linked to Drive and stores a Drive object ID. Base64 format is not processed in this field, so the file must first be uploaded to Bitrix24 Drive

To create a task with a file, perform the following two methods sequentially:

1. [disk.folder.uploadfile](../../api-reference/disk/folder/disk-folder-upload-file.md) — uploads a file to Drive
2. [tasks.task.add](../../api-reference/tasks/tasks-task-add.md) — creates a task
   
## 1. Upload a File to Bitrix24 Drive

To upload a file to Drive, use the [disk.folder.uploadfile](../../api-reference/disk/folder/disk-folder-upload-file.md) method with the following parameters:

* `id` — specify the value of `1739` — the Drive folder ID where the file will be uploaded
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

- Go

    ```go
    // fileContent is the file transport in Bitrix24: an array of two elements,
    // [file name, content in base64]. The request body is JSON anyway, so a regular
    // []string is serialized exactly as the method expects: neither multipart nor manual
    // url encoding is needed. Base64 inflates the data by about a third —
    // this path for small files.
    res, err := core.Call(ctx, "disk.folder.uploadfile", b24.Params{
    	"id":          folderID,
    	"data":        b24.Params{"NAME": "ava555.jpg"},
    	"fileContent": []string{"ava555.jpg", base64.StdEncoding.EncodeToString(content)},
    	// Re-running the example must not fail because of a name collision.
    	"generateUniqueName": true,
    })
    if err != nil {
    	return fmt.Errorf("disk.folder.uploadfile: %w", err)
    }

    var file struct {
    	// ID is the ID of the DRIVE OBJECT, and it is exactly what fields of type
    	// "file (Drive)".
    	ID b24.ID `json:"ID"`
    	// FILE_ID is the internal file ID. If you substitute it into a field
    	// of the task, the file either will not attach or the wrong one will.
    	FileID b24.ID `json:"FILE_ID"`
    	Name   string `json:"NAME"`
    }
    if err := json.Unmarshal(res.Result, &file); err != nil {
    	return fmt.Errorf("parse the uploaded file: %w", err)
    }
    ```

{% endlist %}

As a result of uploading the file to Drive, you will receive two different file ID values:

* `FILE_ID`: `28073` — the internal file ID value
* `ID`: `6687` — the Drive object ID; use this value in methods when working with "File (Drive)" type fields. If you pass the value `FILE_ID` in a request to change a "File (Drive)" field, the file will either not be attached to the task because no Drive object exists with that ID, or the wrong file will be attached

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
## 2. Create a Task with a File

To create a task, use the [tasks.task.add](../../api-reference/tasks/tasks-task-add.md) method with the following parameters:

* `UF_TASK_WEBDAV_FILES` — specify the value of `n6687`. This is the file ID from the previous method's result, to which we add a prefix `n` to upload the file to the field
* `TITLE` — the task name, a required field. The task will not be created without a name
* `CREATED_BY` — the task creator ID; this field cannot be empty. If left blank, the person sending the request will automatically become the creator
* `RESPONSIBLE_ID` — the task assignee ID, a required field. The task will not be created without an assignee

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

- Go

    ```go
    // The "n" prefix before a Drive object ID means "attach exactly
    // this existing object". The method will not accept a bare number. The field is always
    // an array, even when there is a single file.
    res, err = core.Call(ctx, "tasks.task.add", b24.Params{
    	"fields": b24.Params{
    		"TITLE":                "task for test",
    		"CREATED_BY":           userID,
    		"RESPONSIBLE_ID":       userID,
    		"UF_TASK_WEBDAV_FILES": []string{"n" + strconv.FormatInt(int64(file.ID), 10)},
    	},
    })
    if err != nil {
    	return fmt.Errorf("tasks.task.add: %w", err)
    }

    // tasks.* wraps the response in an object with the task key — unlike crm.*.add,
    // which responds with a bare ID. And here the ID arrives
    // AS A STRING ("3711"): b24.ID parses both spellings, a plain int does not.
    var out struct {
    	Task struct {
    		ID    b24.ID `json:"id"`
    		Title string `json:"title"`
    	} `json:"task"`
    }
    if err := json.Unmarshal(res.Result, &out); err != nil {
    	return fmt.Errorf("parse the created task: %w", err)
    }
    ```

{% endlist %}

We have created a task with ID `3711`. 

```json
{
    "result": {
        "task": {
            "id": "3711",
            "parentId": null,
            "title": "task for test",
            "description": "",
            "mark": null,
            "priority": "1",
            "multitask": "N",
            "notViewed": "N",
            "replicate": "N",
            "stageId": "0",
            "createdBy": "1",
            "createdDate": "2024-11-02T10:06:08+02:00",
            "responsibleId": "1",
            "changedBy": "1",
            "changedDate": "2024-11-02T10:06:08+02:00",
            "statusChangedBy": null,
            "closedBy": null,
            "closedDate": null,
            "activityDate": "2024-11-02T10:06:08+02:00",
            "dateStart": null,
            "deadline": null,
            "startDatePlan": null,
            "endDatePlan": null,
            "guid": "{c2794da9-c7fe-404d-a709-ddab4578717a}",
            "xmlId": null,
            "commentsCount": null,
            "serviceCommentsCount": null,
            "allowChangeDeadline": "N",
            "allowTimeTracking": "N",
            "taskControl": "N",
            "addInReport": "N",
            "forkedByTemplateId": null,
            "timeEstimate": "0",
            "timeSpentInLogs": null,
            "matchWorkTime": "N",
            "forumTopicId": null,
            "forumId": null,
            "siteId": "s1",
            "subordinate": "Y",
            "exchangeModified": null,
            "exchangeId": null,
            "outlookVersion": "1",
            "viewedDate": null,
            "sorting": null,
            "durationFact": null,
            "isMuted": "N",
            "isPinned": "N",
            "isPinnedInGroup": "N",
            "flowId": null,
            "descriptionInBbcode": "Y",
            "status": "2",
            "statusChangedDate": "2024-11-02T10:06:08+02:00",
            "durationPlan": null,
            "durationType": "days",
            "favorite": "N",
            "groupId": "0",
            "auditors": [],
            "accomplices": [],
            "checklist": [],
            "group": [],
            "creator": {
                "id": "1",
                "name": "Viola",
                "link": "/company/personal/user/1/",
                "icon": "https://your-domain.bitrix24.com/b13743910/resize_cache/2267/c0120a8d7c10d63c83e32398d1ec4d9e/main/c7b/c7bd44b1babaa5448125dd97d038ce1b/d5fb56b94dc2c3cd8c006a2c595a4895.jpg",
                "workPosition": ""
            },
            "responsible": {
                "id": "1",
                "name": "Viola",
                "link": "/company/personal/user/1/",
                "icon": "https://your-domain.bitrix24.com/b13743910/resize_cache/2267/c0120a8d7c10d63c83e32398d1ec4d9e/main/c7b/c7bd44b1babaa5448125dd97d038ce1b/d5fb56b94dc2c3cd8c006a2c595a4895.jpg",
                "workPosition": ""
            },
            "accomplicesData": [],
            "auditorsData": [],
            "newCommentsCount": 0,
            "action": {
                "accept": false,
                "decline": false,
                "complete": true,
                "approve": false,
                "disapprove": false,
                "start": true,
                "pause": false,
                "delegate": true,
                "remove": true,
                "edit": true,
                "defer": true,
                "renew": false,
                "create": true,
                "changeDeadline": true,
                "checklistAddItems": true,
                "addFavorite": true,
                "deleteFavorite": false,
                "rate": true,
                "edit.originator": false,
                "checklist.reorder": true,
                "elapsedtime.add": true,
                "dayplan.timer.toggle": false,
                "edit.plan": true,
                "checklist.add": true,
                "favorite.add": true,
                "favorite.delete": false
            },
            "checkListTree": {
                "nodeId": 0,
                "fields": {
                    "id": null,
                    "copiedId": null,
                    "entityId": null,
                    "userId": 1,
                    "createdBy": null,
                    "parentId": null,
                    "title": "",
                    "sortIndex": null,
                    "displaySortIndex": "",
                    "isComplete": false,
                    "isImportant": false,
                    "completedCount": 0,
                    "members": [],
                    "attachments": []
                },
                "action": [],
                "descendants": []
            },
            "checkListCanAdd": true
        }
    }
}
```
The resulting output does not contain information about the task's files. To verify whether the file was successfully attached to the task, call the [tasks.task.get](../../api-reference/tasks/tasks-task-get.md) method, specifying the `UF_TASK_WEBDAV_FILES` field in `SELECT`.

As a result of [tasks.task.get](../../api-reference/tasks/tasks-task-get.md), you will receive the ID of the record representing the attachment of the Drive file to the task — this is the link ID that connects the task and the Drive file. To retrieve information about the file using the link ID, use the [disk.attachedObject.get](../../api-reference/disk/attached-object/disk-attached-object-get.md) method. 

## Code Example

{% list tabs %}

- JS

    ```javascript
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/')

    // Function for uploading a file
    async function uploadFileToDisk() {
        // Folder ID where you want to upload the file
        const folderId = 'your_folder_ID';
        // File name and its content in Base64 format
        const fileName = 'your_file_name';
        const fileContentBase64 = 'your_file_content_Base64';

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
        await createTaskWithFile(fileId);
    }

    // Function for creating a task with an attached file
    async function createTaskWithFile(fileId) {
        // Task parameters
        const taskTitle = 'your_task_name';
        const taskDescription = 'your_task_description';
        const responsibleId = 'your_responsible_ID';

        // Calling the tasks.task.add method
        const response = await $b24.actions.v2.call.make({
            method: 'tasks.task.add',
            params: {
                fields: {
                    TITLE: taskTitle,
                    DESCRIPTION: taskDescription,
                    RESPONSIBLE_ID: responsibleId,
                    UF_TASK_WEBDAV_FILES: ['n' + fileId] // Adding prefix 'n' to the file ID
                }
            },
            requestId: 'task-add'
        });

        if (!response.isSuccess) {
            console.error('Error while creating the task:', response.getErrorMessages().join('; '));
            return;
        }

        console.log('Task created successfully!', response.getData().result);
    }

    // Calling the function to upload a file and create a task
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
        // Folder ID where you want to upload the file
        $folderId = 'your_folder_ID';
        // File name that you want to upload
        $fileName = 'your_file_name';
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
        createTaskWithFile($serviceBuilder, $fileId);
    }

    // Function for creating a task with an attached file
    function createTaskWithFile($serviceBuilder, $fileId) {
        // Task parameters
        $taskTitle = 'your_task_name';
        $taskDescription = 'your_task_description';
        $responsibleId = 'your_responsible_ID';

        // Calling the tasks.task.add method
        try {
            $serviceBuilder->core->call(
                'tasks.task.add',
                [
                    'fields' => [
                        'TITLE' => $taskTitle,
                        'DESCRIPTION' => $taskDescription,
                        'RESPONSIBLE_ID' => $responsibleId,
                        'UF_TASK_WEBDAV_FILES' => ['n' . $fileId] // Adding prefix 'n' to the file ID
                    ]
                ]
            );
        } catch (BaseException $e) {
            echo 'Error while creating the task: ' . $e->getMessage();
            return;
        }

        echo 'Task created successfully!';
    }

    // Calling the function to upload a file and create a task
    uploadFileToDisk($serviceBuilder);
    ```

- Python

    ```python
    from b24pysdk import BitrixWebhook, Client
    from b24pysdk.errors import BitrixAPIError

    def upload_file_to_drive(client):
        folder_id = "your_folder_ID"
        file_name = "your_file_name"
        file_content_base64 = "your_file_content_Base64"

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
            create_task_with_file(client, file_id)

    def create_task_with_file(client, file_id):
        task_title = "your_task_name"
        task_description = "your_task_description"
        responsible_id = "your_responsible_ID"

        try:
            client.tasks.task.add(
                fields={
                    "TITLE": task_title,
                    "DESCRIPTION": task_description,
                    "RESPONSIBLE_ID": responsible_id,
                    "UF_TASK_WEBDAV_FILES": [f"n{file_id}"],
                },
            ).response
        except BitrixAPIError as error:
            print(f"Task creation error: {error}")
        else:
            print("Task created successfully!")

    client = Client(
        BitrixWebhook(
            domain="your-domain.bitrix24.com",
            webhook_token="user_id/webhook_key",
        )
    )

    upload_file_to_drive(client)
    ```

- Go

    ```go
    // Setup in an empty directory — go get will not work without go mod init:
    //
    //	go mod init example && go get github.com/bitrix24/b24gosdk
    //
    // Run:
    //
    //	export B24_WEBHOOK_URL='https://your-portal.bitrix24.com/rest/1/token/' && go run .
    //
    // The example is self-contained: it finds a folder on Drive itself, uploads a file there,
    // creates a task with this file, verifies the attachment, and cleans up after itself.
    // It runs on any portal, nothing needs to be edited.
    package main

    import (
    	"context"
    	"encoding/base64"
    	"encoding/json"
    	"fmt"
    	"log"
    	"os"
    	"strconv"

    	b24 "github.com/bitrix24/b24gosdk"
    )

    func main() {
    	if err := run(context.Background()); err != nil {
    		log.Fatal(err)
    	}
    }

    func run(ctx context.Context) error {
    	// The webhook path is a secret, so it comes from the environment, not from the code.
    	core := b24.NewClient(os.Getenv("B24_WEBHOOK_URL")).Core()

    	// --- setup: whose Drive and which folder

    	userID, err := currentUser(ctx, core)
    	if err != nil {
    		return err
    	}
    	folderID, err := rootFolder(ctx, core, userID)
    	if err != nil {
    		return err
    	}

    	// --- step 1: upload the file to Drive

    	content := []byte("Quarterly report.\nCreated by the b24gosdk example.\n")
    	// fileContent is the file transport in Bitrix24: an array of two elements,
    	// [file name, content in base64]. The request body is JSON anyway, so a regular
    	// []string is serialized exactly as the method expects: neither multipart nor manual
    	// url encoding is needed. Base64 inflates the data by about a third —
    	// this path for small files.
    	res, err := core.Call(ctx, "disk.folder.uploadfile", b24.Params{
    		"id":          folderID,
    		"data":        b24.Params{"NAME": "ava555.jpg"},
    		"fileContent": []string{"ava555.jpg", base64.StdEncoding.EncodeToString(content)},
    		// Re-running the example must not fail because of a name collision.
    		"generateUniqueName": true,
    	})
    	if err != nil {
    		return fmt.Errorf("disk.folder.uploadfile: %w", err)
    	}

    	var file struct {
    		// ID is the ID of the DRIVE OBJECT, and it is exactly what fields of type
    		// "file (Drive)".
    		ID b24.ID `json:"ID"`
    		// FILE_ID is the internal file ID. If you substitute it into a field
    		// of the task, the file either will not attach or the wrong one will.
    		FileID b24.ID `json:"FILE_ID"`
    		Name   string `json:"NAME"`
    	}
    	if err := json.Unmarshal(res.Result, &file); err != nil {
    		return fmt.Errorf("parse the uploaded file: %w", err)
    	}
    	defer del(ctx, core, "disk.file.delete", b24.Params{"id": file.ID})
    	fmt.Printf("file %q uploaded: ID=%d, FILE_ID=%d\n", file.Name, file.ID, file.FileID)

    	// --- step 2: create a task with this file
    	// The "n" prefix before a Drive object ID means "attach exactly
    	// this existing object". The method will not accept a bare number. The field is always
    	// an array, even when there is a single file.
    	res, err = core.Call(ctx, "tasks.task.add", b24.Params{
    		"fields": b24.Params{
    			"TITLE":                "task for test",
    			"CREATED_BY":           userID,
    			"RESPONSIBLE_ID":       userID,
    			"UF_TASK_WEBDAV_FILES": []string{"n" + strconv.FormatInt(int64(file.ID), 10)},
    		},
    	})
    	if err != nil {
    		return fmt.Errorf("tasks.task.add: %w", err)
    	}

    	// tasks.* wraps the response in an object with the task key — unlike crm.*.add,
    	// which responds with a bare ID. And here the ID arrives
    	// AS A STRING ("3711"): b24.ID parses both spellings, a plain int does not.
    	var out struct {
    		Task struct {
    			ID    b24.ID `json:"id"`
    			Title string `json:"title"`
    		} `json:"task"`
    	}
    	if err := json.Unmarshal(res.Result, &out); err != nil {
    		return fmt.Errorf("parse the created task: %w", err)
    	}
    	defer del(ctx, core, "tasks.task.delete", b24.Params{"taskId": out.Task.ID})
    	fmt.Printf("task %d %q created\n", out.Task.ID, out.Task.Title)

    	// --- check: the file is actually attached

    	return checkAttachment(ctx, core, out.Task.ID)
    }

    // --- helpers: data setup, verification, and cleanup

    func currentUser(ctx context.Context, core *b24.Core) (b24.ID, error) {
    	res, err := core.Call(ctx, "user.current", nil, b24.WithIdempotent())
    	if err != nil {
    		return 0, fmt.Errorf("user.current: %w", err)
    	}
    	var u struct {
    		ID b24.ID `json:"ID"`
    	}
    	if err := json.Unmarshal(res.Result, &u); err != nil {
    		return 0, err
    	}
    	return u.ID, nil
    }

    // rootFolder returns the root folder of the user's personal storage, and if
    // it does not exist — the shared portal storage. The page substitutes a ready-made number here
    // of the folder; on someone else's portal such a number does not exist, so the example looks it up.
    func rootFolder(ctx context.Context, core *b24.Core, userID b24.ID) (b24.ID, error) {
    	for _, filter := range []b24.Params{
    		{"ENTITY_TYPE": "user", "ENTITY_ID": userID},
    		{"ENTITY_TYPE": "common"},
    	} {
    		res, err := core.Call(ctx, "disk.storage.getlist",
    			b24.Params{"filter": filter}, b24.WithIdempotent())
    		if err != nil {
    			return 0, fmt.Errorf("disk.storage.getlist: %w", err)
    		}
    		var storages []struct {
    			Name         string `json:"NAME"`
    			RootObjectID b24.ID `json:"ROOT_OBJECT_ID"`
    		}
    		if err := json.Unmarshal(res.Result, &storages); err != nil {
    			return 0, err
    		}
    		for _, s := range storages {
    			if s.RootObjectID != 0 {
    				fmt.Printf("storage %q, root folder %d\n", s.Name, s.RootObjectID)
    				return s.RootObjectID, nil
    			}
    		}
    	}
    	return 0, fmt.Errorf("the webhook cannot see any storage on Drive")
    }

    // checkAttachment demonstrates what the page describes: in the tasks.task.add response
    // there is no information about the files, it has to be requested separately.
    func checkAttachment(ctx context.Context, core *b24.Core, taskID b24.ID) error {
    	res, err := core.Call(ctx, "tasks.task.get", b24.Params{
    		"taskId": taskID,
    		"select": []string{"ID", "TITLE", "UF_TASK_WEBDAV_FILES"},
    	}, b24.WithIdempotent())
    	if err != nil {
    		return fmt.Errorf("tasks.task.get: %w", err)
    	}

    	// The portal responds with a different name than the one requested: UF_TASK_WEBDAV_FILES
    	// arrives as ufTaskWebdavFiles. UnwrapFold compares names ignoring
    	// case and underscores, so renaming does not break it.
    	raw, ok := b24.UnwrapFold(res.Result, "task", "UF_TASK_WEBDAV_FILES")
    	if !ok || b24.IsEmpty(raw) {
    		return fmt.Errorf("the file was not attached to task %d", taskID)
    	}

    	// The value is the list of IDs of the LINK between the task and the Drive file, not
    	// the IDs of the files themselves.
    	var attachIDs []b24.ID
    	if err := json.Unmarshal(raw, &attachIDs); err != nil {
    		return fmt.Errorf("parse attachments: %w", err)
    	}
    	fmt.Printf("attachments attached to the task: %d (link IDs %v)\n",
    		len(attachIDs), attachIDs)
    	return nil
    }

    // del removes what was created. A cleanup error is printed but not returned: it must not
    // mask the real error of the scenario.
    func del(ctx context.Context, core *b24.Core, method string, params b24.Params) {
    	if _, err := core.Call(ctx, method, params); err != nil {
    		fmt.Fprintf(os.Stderr, "cleanup, %s: %v
", method, err)
    	}
    }
    ```

{% endlist %}

## Continue Learning

* [How to Upload a File to a Task](./how-to-upload-file-to-task.md)
