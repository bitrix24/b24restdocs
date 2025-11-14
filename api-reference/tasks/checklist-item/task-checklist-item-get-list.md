# Get the list of checklist items task.checklistitem.getlist

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user with read access permission for the task or higher

The method `task.checklistitem.getlist` retrieves a list of checklist items in a task.

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **TASKID***
[`integer`](../../data-types.md) | Task identifier.

The identifier can be obtained when [creating a task](../tasks-task-add.md) or by using the [get task list method](../tasks-task-list.md) ||
|| **ORDER**
[`object`](../../data-types.md) | An object for sorting the result in the form `{"field": "sort value", ... }`.

You can sort by the following fields:
- `ID` — checklist item identifier
- `PARENT_ID` — parent item identifier
- `CREATED_BY` — identifier of the item author
- `TITLE` — text of the checklist item
- `SORT_INDEX` — sort index
- `IS_COMPLETE` — completion status of the item
- `IS_IMPORTANT` — importance mark of the item
- `TOGGLED_BY` — identifier of the user who last changed the item's status
- `TOGGLED_DATE` — date and time of the item's status change

The sort direction can take the following values:
- `asc` — ascending
- `desc` — descending

By default, the result is sorted by `ID` in descending order ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASKID":8017,"ORDER":{"IS_COMPLETE":"ASC"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/task.checklistitem.getlist
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASKID":8017,"ORDER":{"IS_COMPLETE":"ASC"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/task.checklistitem.getlist
    ```

- JS

    ```javascript
    // callListMethod: Retrieves all data at once.
    // Use only for small selections (< 1000 items) due to high
    // memory load.

    try {
    const response = await $b24.callListMethod(
        'task.checklistitem.getlist',
        {
        TASKID: 8017,
        ORDER: {
            IS_COMPLETE: 'ASC'
        }
        },
        (progress: number) => { console.log('Progress:', progress) }
    );
    const items = response.getData() || [];
    for (const entity of items) { console.log('Entity:', entity) }
    } catch (error: any) {
    console.error('Request failed', error)
    }

    // fetchListMethod: Retrieves data in parts using an iterator.
    // Use for large volumes of data for efficient memory consumption.

    try {
    const generator = $b24.fetchListMethod('task.checklistitem.getlist', {
        TASKID: 8017,
        ORDER: {
        IS_COMPLETE: 'ASC'
        }
    }, 'ID');
    for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity) }
    }
    } catch (error: any) {
    console.error('Request failed', error)
    }

    // callMethod: Manual control of pagination through the start parameter.
    // Use for precise control over request batches.
    // Less efficient for large data than fetchListMethod.

    try {
    const response = await $b24.callMethod('task.checklistitem.getlist', {
        TASKID: 8017,
        ORDER: {
        IS_COMPLETE: 'ASC'
        }
    }, 0);
    const result = response.getData().result || [];
    for (const entity of result) { console.log('Entity:', entity) }
    } catch (error: any) {
    console.error('Request failed', error)
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'task.checklistitem.getlist',
                [
                    'TASKID' => 8017,
                    'ORDER' => [
                        'IS_COMPLETE' => 'ASC'
                    ]
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching checklist items: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'task.checklistitem.getlist',
        {
            'TASKID': 8017,
            'ORDER': {
                'IS_COMPLETE': 'ASC'
            }
        },
        function(result){
            console.info(result.data());
            console.log(result);
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'task.checklistitem.getlist',
        [
            'TASKID' => 8017,
            'ORDER' => [
                'IS_COMPLETE' => 'ASC'
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": [
        {
            "ID": "477",
            "TASK_ID": "8017",
            "PARENT_ID": "431",
            "CREATED_BY": "503",
            "TITLE": "Prepare contract Sarah Johnson",
            "SORT_INDEX": "2",
            "IS_COMPLETE": "N",
            "IS_IMPORTANT": "N",
            "TOGGLED_BY": "503",
            "TOGGLED_DATE": "2025-11-10T15:02:30+02:00",
            "MEMBERS": [
                {
                    "ID": "103",
                    "TYPE": "A",
                    "NAME": "Sarah Johnson",
                    "PERSONAL_PHOTO": "8644",
                    "PERSONAL_GENDER": "F",
                    "IMAGE": "https://mysite.com/b17053/resize_cache/8644/c0120a8d7c10d63c83e32398d1ec4d9e/main/45f/45fff10d17d398a5583184c8350cd197/buh.jpg",
                    "IS_COLLABER": false
                }
            ],
            "ATTACHMENTS": {
                "1113": {
                    "ATTACHMENT_ID": 1113,
                    "NAME": "Instructions.docx",
                    "SIZE": "115161",
                    "FILE_ID": "5065",
                    "DOWNLOAD_URL": "/bitrix/tools/disk/uf.php?attachedId=1113&action=download&ncc=1",
                    "VIEW_URL": "/bitrix/tools/disk/uf.php?attachedId=1113&action=show&ncc=1"
                },
                "1115": {
                    "ATTACHMENT_ID": 1115,
                    "NAME": "Document list.xlsx",
                    "SIZE": "14675",
                    "FILE_ID": "5067",
                    "DOWNLOAD_URL": "/bitrix/tools/disk/uf.php?attachedId=1115&action=download&ncc=1",
                    "VIEW_URL": "/bitrix/tools/disk/uf.php?attachedId=1115&action=show&ncc=1"
                }
            }
        },
        {
            "ID": "431",
            "TASK_ID": "8017",
            "PARENT_ID": 0,
            "CREATED_BY": "503",
            "TITLE": "Checklist 1",
            "SORT_INDEX": "0",
            "IS_COMPLETE": "N",
            "IS_IMPORTANT": "N",
            "TOGGLED_BY": null,
            "TOGGLED_DATE": "",
            "MEMBERS": [],
            "ATTACHMENTS": []
        },
        {
            "ID": "447",
            "TASK_ID": "8017",
            "PARENT_ID": "431",
            "CREATED_BY": "503",
            "TITLE": "Agree on details with the client",
            "SORT_INDEX": "1",
            "IS_COMPLETE": "N",
            "IS_IMPORTANT": "N",
            "TOGGLED_BY": null,
            "TOGGLED_DATE": "",
            "MEMBERS": [],
            "ATTACHMENTS": []
        },
        {
            "ID": "469",
            "TASK_ID": "8017",
            "PARENT_ID": "447",
            "CREATED_BY": "503",
            "TITLE": "Agree with the manager Andrew Smith Andrew Johnson",
            "SORT_INDEX": "2",
            "IS_COMPLETE": "N",
            "IS_IMPORTANT": "N",
            "TOGGLED_BY": null,
            "TOGGLED_DATE": "",
            "MEMBERS": [
                {
                    "ID": "3",
                    "TYPE": "A",
                    "NAME": "Andrew Smith",
                    "PERSONAL_PHOTO": "249",
                    "PERSONAL_GENDER": "M",
                    "IMAGE": "https://mysite.com/b17053/resize_cache/249/c0120a8d7c10d63c83e32398d1ec4d9e/main/cd526b0644e7ff4d794ea41cb36bc423/odmin.png",
                    "IS_COLLABER": false
                },
                {
                    "ID": "11",
                    "TYPE": "U",
                    "NAME": "Andrew Johnson",
                    "PERSONAL_PHOTO": "231",
                    "PERSONAL_GENDER": "",
                    "IMAGE": "https://mysite.com/b17053/resize_cache/231/c0120a8d7c10d63c83e32398d1ec4d9e/main/026bf59e161a0bd50f401d3796800651/66b.jpg",
                    "IS_COLLABER": false
                }
            ],
            "ATTACHMENTS": []
        },
        {
            "ID": "471",
            "TASK_ID": "8017",
            "PARENT_ID": "447",
            "CREATED_BY": "503",
            "TITLE": "Prepare solution",
            "SORT_INDEX": "1",
            "IS_COMPLETE": "N",
            "IS_IMPORTANT": "N",
            "TOGGLED_BY": null,
            "TOGGLED_DATE": "",
            "MEMBERS": [],
            "ATTACHMENTS": []
        },
        {
            "ID": "491",
            "TASK_ID": "8017",
            "PARENT_ID": "431",
            "CREATED_BY": "503",
            "TITLE": "Sign contract",
            "SORT_INDEX": "3",
            "IS_COMPLETE": "N",
            "IS_IMPORTANT": "N",
            "TOGGLED_BY": null,
            "TOGGLED_DATE": "",
            "MEMBERS": [],
            "ATTACHMENTS": []
        },
        {
            "ID": "433",
            "TASK_ID": "8017",
            "PARENT_ID": "431",
            "CREATED_BY": "503",
            "TITLE": "Find all documents for the client",
            "SORT_INDEX": "0",
            "IS_COMPLETE": "Y",
            "IS_IMPORTANT": "N",
            "TOGGLED_BY": "503",
            "TOGGLED_DATE": "2025-11-10T15:02:30+02:00",
            "MEMBERS": [],
            "ATTACHMENTS": []
        },
        {
            "ID": "485",
            "TASK_ID": "8017",
            "PARENT_ID": "447",
            "CREATED_BY": "503",
            "TITLE": "Arrange a meeting Andrew Johnson",
            "SORT_INDEX": "0",
            "IS_COMPLETE": "Y",
            "IS_IMPORTANT": "N",
            "TOGGLED_BY": "503",
            "TOGGLED_DATE": "2025-11-10T15:02:33+02:00",
            "MEMBERS": [
                {
                    "ID": "11",
                    "TYPE": "U",
                    "NAME": "Andrew Johnson",
                    "PERSONAL_PHOTO": "231",
                    "PERSONAL_GENDER": "",
                    "IMAGE": "https://mysite.com/b17053/resize_cache/231/c0120a8d7c10d63c83e32398d1ec4d9e/main/026bf59e161a0bd50f401d3796800651/66b.jpg",
                    "IS_COLLABER": false
                }
            ],
            "ATTACHMENTS": []
        }
    ],
    "time": {
        "start": 1762780903,
        "finish": 1762780903.978847,
        "duration": 0.9788470268249512,
        "processing": 0,
        "date_start": "2025-11-10T16:21:43+02:00",
        "date_finish": "2025-11-10T16:21:43+02:00",
        "operating_reset_at": 1762781503,
        "operating": 0.3446669578552246
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | A list of objects with [description of checklist item fields](#result-fields) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Fields of the result object {#result-fields}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`string`](../../data-types.md) | Checklist item identifier ||
|| **TASK_ID**
[`string`](../../data-types.md) | Identifier of the task to which the item belongs ||
|| **PARENT_ID**
[`string`](../../data-types.md) | Identifier of the parent item.

A value of `0` indicates a root item ||
|| **CREATED_BY**
[`string`](../../data-types.md) | Identifier of the item author ||
|| **TITLE**
[`string`](../../data-types.md) | Text of the checklist item.

If `PARENT_ID = 0`, the field contains the name of the checklist ||
|| **SORT_INDEX**
[`string`](../../data-types.md) | Sort index.

The smaller the value, the higher the item in the list or sublist ||
|| **IS_COMPLETE**
[`boolean`](../../data-types.md) | Completion status of the item. Possible values:
- `Y` — completed,
- `N` — not completed ||
|| **IS_IMPORTANT**
[`boolean`](../../data-types.md) | Importance mark of the item. Possible values:
- `Y` — important,
- `N` — ordinary ||
|| **TOGGLED_BY**
[`string`](../../data-types.md) | Identifier of the user who last changed the item's status.

Can be `null` if the status has not been changed ||
|| **TOGGLED_DATE**
[`string`](../../data-types.md) | Date and time of the item's status change in `ISO 8601` format ||
|| **MEMBERS**
[`array`](../../data-types.md) | A list of objects with [description of participants](#members) ||
|| **ATTACHMENTS**
[`object`](../../data-types.md) | An object with [description of attached files](#attachments).

The key is the attachment file identifier `ATTACHMENT_ID` ||
|#

#### Members Object {#members}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`string`](../../data-types.md) | User identifier ||
|| **TYPE**
[`string`](../../data-types.md) | User's role in the checklist item. Possible values:
- `A` — participant,
- `U` — observer ||
|| **NAME**
[`string`](../../data-types.md) | User's name ||
|| **PERSONAL_PHOTO**
[`string`](../../data-types.md) | Identifier of the user's avatar file on Drive ||
|| **PERSONAL_GENDER**
[`string`](../../data-types.md) | User's gender. Possible values:
- `M` — male,
- `F` — female ||
|| **IMAGE**
[`string`](../../data-types.md) | Link to the user's avatar ||
|| **IS_COLLABER**
[`boolean`](../../data-types.md) | Indicates that the user is an external participant ||
|#

#### Attachments Object {#attachments}

#|
|| **Name**
`type` | **Description** ||
|| **ATTACHMENT_ID**
[`integer`](../../data-types.md) | Attachment identifier ||
|| **NAME**
[`string`](../../data-types.md) | File name ||
|| **SIZE**
[`string`](../../data-types.md) | File size in bytes ||
|| **FILE_ID**
[`string`](../../data-types.md) | File identifier on Drive ||
|| **DOWNLOAD_URL**
[`string`](../../data-types.md) | Link to download the file ||
|| **VIEW_URL**
[`string`](../../data-types.md) | Link to view the file in the browser ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":"ERROR_CORE",
    "error_description":"TASKS_ERROR_EXCEPTION_#8; Action failed; 8\/TE\/ACTION_FAILED_TO_BE_PROCESSED\u003Cbr\u003E"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value**  ||
|| `ERROR_CORE` | TASKS_ERROR_EXCEPTION_#8; Action failed; 8\/TE\/ACTION_FAILED_TO_BE_PROCESSED\u003Cbr\u003E | The user does not have access to the task ||
|| `ERROR_CORE` | TASKS_ERROR_EXCEPTION_#256; Param #0 (taskId) for method ctaskchecklistitem::getlist() expected to be of type \u0022integer\u0022, but given something else.; 256\/TE\/WRONG_ARGUMENTS\u003Cbr\u003E | The required parameter `TASKID` is not provided or an incorrect type is specified ||
|| `ERROR_CORE` | TASKS_ERROR_EXCEPTION_#256; Param #1 (arOrder) for method ctaskchecklistitem::getlist() must not contain key \u0022IS_COMPLETED\u0022.; 256\/TE\/WRONG_ARGUMENTS\u003Cbr\u003E | An invalid field is specified in `ORDER` ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./task-checklist-item-add.md)
- [{#T}](./task-checklist-item-update.md)
- [{#T}](./task-checklist-item-get.md)
- [{#T}](./task-checklist-item-delete.md)
- [{#T}](./task-checklist-item-move-after-item.md)
- [{#T}](./task-checklist-item-complete.md)
- [{#T}](./task-checklist-item-renew.md)
- [{#T}](./task-checklist-item-is-action-allowed.md)