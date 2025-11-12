# Get checklist item task.checklistitem.get

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user with read access permission for the task or higher

The method `task.checklistitem.get` retrieves the description of a checklist item by its identifier.

## Method Parameters

{% note warning "" %}

Pass parameters in the request according to the order in the table. If the order is violated, the request will return an error.

{% endnote %}

{% include [Footnote about parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **TASKID***
[`integer`](../../data-types.md) | Task identifier.

The identifier can be obtained when [creating a task](../tasks-task-add.md) or by using the [method to get the list of tasks](../tasks-task-list.md) ||
|| **ITEMID***
[`integer`](../../data-types.md) | Checklist item identifier.

The item identifier can be obtained when [adding a new item](./task-checklist-item-add.md) or by using the [method to get the list of checklist items](./task-checklist-item-get-list.md) ||
|#

## Code Examples

{% include [Footnote about examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASKID":8017,"ITEMID":479}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/task.checklistitem.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASKID":8017,"ITEMID":479,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/task.checklistitem.get
    ```

- JS

    ```javascript
    try
    {
        const response = await $b24.callMethod(
            'task.checklistitem.get',
            {
                TASKID: 8017,
                ITEMID: 479
            }
        );
        
        const result = response.getData().result;
        console.log('Retrieved checklist item:', result);
        processResult(result);
    }
    catch( error )
    {
        console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'task.checklistitem.get',
                [
                    'TASKID' => 8017,
                    'ITEMID' => 479
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error retrieving checklist item: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'task.checklistitem.get',
        {
            TASKID: 8017,
            ITEMID: 479
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
        'task.checklistitem.get',
        [
            'TASKID' => 8017,
            'ITEMID' => 479
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
    "result": {
        "ID": "495",
        "TASK_ID": "8017",
        "PARENT_ID": "457",
        "CREATED_BY": "503",
        "TITLE": "Prepare report Andrew Smith Sarah Johnson Andrew Brown",
        "SORT_INDEX": "4",
        "IS_COMPLETE": "N",
        "IS_IMPORTANT": "Y",
        "TOGGLED_BY": null,
        "TOGGLED_DATE": "",
        "MEMBERS": [
            {
                "ID": "3",
                "TYPE": "U",
                "NAME": "Andrew Brown",
                "PERSONAL_PHOTO": "249",
                "PERSONAL_GENDER": "M",
                "IMAGE": "https://mysite.com/b17053/resize_cache/249/c0120a8d7c10d63c83e32398d1ec4d9e/main/cd526b0644e7ff4d794ea41cb36bc423/odmin.png",
                "IS_COLLABER": false
            },
            {
                "ID": "11",
                "TYPE": "U",
                "NAME": "Andrew Smith",
                "PERSONAL_PHOTO": "231",
                "PERSONAL_GENDER": "M",
                "IMAGE": "https://mysite.com/b17053/resize_cache/231/c0120a8d7c10d63c83e32398d1ec4d9e/main/026bf59e161a0bd50f401d3796800651/66b.jpg",
                "IS_COLLABER": false
            },
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
            "1111": {
                "ATTACHMENT_ID": 1111,
                "NAME": "Invoice for client.pdf",
                "SIZE": "148238",
                "FILE_ID": "989",
                "DOWNLOAD_URL": "/bitrix/tools/disk/uf.php?attachedId=1111&action=download&ncc=1",
                "VIEW_URL": "/bitrix/tools/disk/uf.php?attachedId=1111&action=show&ncc=1"
            }
        }
    },
    "time": {
        "start": 1762755387,
        "finish": 1762755387.104804,
        "duration": 0.10480403900146484,
        "processing": 0,
        "date_start": "2025-11-10T09:16:27+02:00",
        "date_finish": "2025-11-10T09:16:27+02:00",
        "operating_reset_at": 1762755987,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Object with [description of checklist item fields](#result-fields) ||
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
[`boolean`](../../data-types.md) | Status of the item. Possible values:
- `Y` — completed,
- `N` — not completed ||
|| **IS_IMPORTANT**
[`boolean`](../../data-types.md) | Importance mark of the item. Possible values:
- `Y` — important,
- `N` — ordinary ||
|| **TOGGLED_BY**
[`string`](../../data-types.md) | Identifier of the user who last changed the status of the item.

Can be `null` if the status has not been changed ||
|| **TOGGLED_DATE**
[`string`](../../data-types.md) | Date and time of the status change in `ISO 8601` format ||
|| **MEMBERS**
[`array`](../../data-types.md) | List of objects with [description of participants](#members) ||
|| **ATTACHMENTS**
[`object`](../../data-types.md) | Object with [description of attached files](#attachments).

Key — identifier of the file attachment `ATTACHMENT_ID` ||
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
[`integer`](../../data-types.md) | Identifier of the attachment ||
|| **NAME**
[`string`](../../data-types.md) | File name ||
|| **SIZE**
[`string`](../../data-types.md) | File size in bytes ||
|| **FILE_ID**
[`string`](../../data-types.md) | Identifier of the file on Drive ||
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
    "error_description":"TASKS_ERROR_EXCEPTION_#256; Param #1 (itemId) expected by method ctaskchecklistitem::get(), but not given.; 256\/TE\/WRONG_ARGUMENTS\u003Cbr\u003E"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value**  ||
|| `ERROR_CORE` | TASKS_ERROR_EXCEPTION_#256; Param #1 (itemId) expected by method ctaskchecklistitem::get(), but not given.; 256\/TE\/WRONG_ARGUMENTS\u003Cbr\u003E | Required parameters `TASKID` and `ITEMID` not provided  ||
|| `ERROR_CORE` | error_description":"TASKS_ERROR_EXCEPTION_#256; Param #0 (taskId) for method ctaskchecklistitem::get() expected to be of type \u0022integer\u0022, but given something else.; 256\/TE\/WRONG_ARGUMENTS\u003Cbr\u003E | Incorrect type of value for `TASKID` or `ITEMID` ||
|| `ERROR_CORE` | TASKS_ERROR_EXCEPTION_#512; Check listitem not found or not accessible; 512\/TE\/ITEM_NOT_FOUND_OR_NOT_ACCESSIBLE\u003Cbr\u003E | Possible reasons:
- the order of parameters in the method is violated
- the specified `TASKID` or `ITEMID` does not exist
- the user does not have access permission to the task ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./task-checklist-item-add.md)
- [{#T}](./task-checklist-item-update.md)
- [{#T}](./task-checklist-item-get-list.md)
- [{#T}](./task-checklist-item-delete.md)
- [{#T}](./task-checklist-item-move-after-item.md)
- [{#T}](./task-checklist-item-complete.md)
- [{#T}](./task-checklist-item-renew.md)
- [{#T}](./task-checklist-item-is-action-allowed.md)