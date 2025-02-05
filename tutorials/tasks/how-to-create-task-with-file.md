# How to Create a Task with an Attached File

> Scope: [`disk`, `tasks`](../../api-reference/scopes/permissions.md)
>
> Who can execute the method: users with access to the disk and tasks sections

In Bitrix24, there are two types of file fields:

* **File.** This field is not linked to the disk; files are uploaded directly through the [Base64 format string](../../api-reference/bx24-js-sdk/how-to-call-rest-methods/files.md).
* **File (disk).** This field is linked to the disk, storing the ID of the disk object. The Base64 format is not processed in this field, so the file must first be uploaded to the Bitrix24 disk.

To create a task with a file, we will sequentially execute two methods:

1. [disk.folder.uploadfile](../../api-reference/disk/folder/disk-folder-upload-file.md) — this method uploads a file to the disk.
2. [tasks.task.add](../../api-reference/tasks/tasks-task-add.md) — this method creates a task.

## 1. Uploading a File to the Bitrix24 Disk

To upload a file to the disk, we use the [disk.folder.uploadfile](../../api-reference/disk/folder/disk-folder-upload-file.md) method with the following parameters:

* `id` — we will specify the value `1739` — the identifier of the disk folder where we are uploading the file.
* `data` — we will specify the file name `NAME`, under which the file will be saved on the Bitrix24 disk.
* `fileContent` — we pass the file in the format ['file_name.extension', 'file as a Base64 encoded string'].

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
* `ID`: `6687` — the disk object ID, which we use in methods for working with fields of the "file (disk)" type. If the request to change the "file (disk)" field passes the `FILE_ID` value, the file will either not be attached to the task because there is no disk object with that ID, or the wrong file will be attached.

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

## 2. Creating a Task with a File

To create a task, we use the [tasks.task.add](../../api-reference/tasks/tasks-task-add.md) method with the following parameters:

* `UF_TASK_WEBDAV_FILES` — we will specify the value `n6687`. This is the file ID from the result of the previous method, to which we add the prefix `n` for uploading the file to the field.
* `TITLE` — the task title, a required field. Without a title, the task will not be created.
* `CREATED_BY` — the ID of the task Creator, this field cannot be empty. If it is not filled, the Creator will automatically be the one who sends the request.
* `RESPONSIBLE_ID` — the ID of the task Participant, a required field. Without a Participant, the task will not be created.

{% list tabs %}

- JS

    ```javascript
    BX24.callMethod(
        'tasks.task.add',
        {
            fields: {
                TITLE: 'task for test',
                RESPONSIBLE_ID: 1,
                UF_TASK_WEBDAV_FILES: [
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
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

We created a task with ID `3711`.

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

In the resulting data, there is no information about the files attached to the task. To check if the file was successfully attached to the task, we will execute the [tasks.task.get](../../api-reference/tasks/tasks-task-get.md) method with the `UF_TASK_WEBDAV_FILES` field in `SELECT`.

As a result of [tasks.task.get](../../api-reference/tasks/tasks-task-get.md), we will obtain the ID of the record linking the disk file to the task — this is the ID of the connection that links the task and the disk file. To get information about the file by the connection ID, we use the [disk.attachedObject.get](../../api-reference/disk/attached-object/disk-attached-object-get.md) method.

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
                    createTaskWithFile(fileId);
                }
            }
        );
    }

    // Function to create a task with an attached file
    function createTaskWithFile(fileId) {
        // Task parameters
        var taskTitle = 'your_task_title';
        var taskDescription = 'your_task_description';
        var responsibleId = 'your_responsible_ID';

        // Call the tasks.task.add method
        BX24.callMethod(
            'tasks.task.add',
            {
                fields: {
                    TITLE: taskTitle,
                    DESCRIPTION: taskDescription,
                    RESPONSIBLE_ID: responsibleId,
                    UF_TASK_WEBDAV_FILES: ['n' + fileId] // Add prefix 'n' to the file ID
                }
            },
            function(result) {
                if (result.error()) {
                    console.error('Error creating task:', result.error());
                } else {
                    console.log('Task successfully created!', result.data());
                }
            }
        );
    }

    // Call the function to upload the file and create the task
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
            createTaskWithFile($fileId);
        }
    }

    // Function to create a task with an attached file
    function createTaskWithFile($fileId) {
        // Task parameters
        $taskTitle = 'your_task_title';
        $taskDescription = 'your_task_description';
        $responsibleId = 'your_responsible_ID';

        // Call the tasks.task.add method
        $result = CRest::call(
            'tasks.task.add',
            [
                'fields' => [
                    'TITLE' => $taskTitle,
                    'DESCRIPTION' => $taskDescription,
                    'RESPONSIBLE_ID' => $responsibleId,
                    'UF_TASK_WEBDAV_FILES' => ['n' . $fileId] // Add prefix 'n' to the file ID
                ]
            ]
        );

        if (isset($result['error'])) {
            echo 'Error creating task: ' . $result['error'];
        } else {
            echo 'Task successfully created!';
        }
    }

    // Call the function to upload the file and create the task
    uploadFileToDisk();
    ```

{% endlist %}

## Continue Learning

* [How to Upload a File to a Task](./how-to-upload-file-to-task.md)