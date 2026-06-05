# Get the List of Checklist Items task.checklistitem.getlist

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user with read access permission to the task or higher.

The method `task.checklistitem.getlist` retrieves a list of checklist items in a task.

## Method Parameters

{% include [Parameter Note](../../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **TASKID*** 
[`integer`](../../data-types.md) | Identifier of the task.

The identifier can be obtained when [creating a task](../tasks-task-add.md) or by using the [get task list method](../tasks-task-list.md) ||
|| **ORDER** 
[`object`](../../data-types.md) | An object for sorting the result in the form `{"field": "sort value", ... }`.

You can sort by the following fields:
- `ID` — identifier of the checklist item
- `PARENT_ID` — identifier of the parent item
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

{% include [Examples Note](../../../_includes/examples.md) %}

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

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type ChecklistItem = {
      ID: string
      TASK_ID: string
      PARENT_ID: string
      CREATED_BY: string
      TITLE: string
      SORT_INDEX: string
      IS_COMPLETE: 'Y' | 'N'
      IS_IMPORTANT: 'Y' | 'N'
      TOGGLED_BY: string | null
      TOGGLED_DATE: ISODate | null
      MEMBERS: Array<{
        ID: string
        TYPE: string
        NAME: string
        PERSONAL_PHOTO: string
        PERSONAL_GENDER: string
        IMAGE: string
        IS_COLLABER: boolean
      }>
      ATTACHMENTS: Record<string, {
        ATTACHMENT_ID: number
        NAME: string
        SIZE: string
        FILE_ID: string
        DOWNLOAD_URL: string
        VIEW_URL: string
      }>
    }

    try {
      // task.checklistitem.getlist returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<ChecklistItem[]>({
        method: 'task.checklistitem.getlist',
        params: {
          TASKID: 8017,
          ORDER: {
            IS_COMPLETE: 'ASC',
          },
          start: 0,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Checklist items count:', result.length, 'First item:', result[0]?.ID, result[0]?.TITLE)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function getChecklistItems() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // task.checklistitem.getlist returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'task.checklistitem.getlist',
            params: {
              TASKID: 8017,
              ORDER: {
                IS_COMPLETE: 'ASC',
              },
              start: 0,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Checklist items count:', result.length, 'First item:', result[0]?.ID, result[0]?.TITLE)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getChecklistItems)
    </script>
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

HTTP Status: **200**

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
                    "NAME": "Document List.xlsx",
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
            "TITLE": "Agree with the manager Andrew Smith John Doe",
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
                    "NAME": "John Doe",
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
            "TITLE": "Arrange a meeting John Doe",
            "SORT_INDEX": "0",
            "IS_COMPLETE": "Y",
            "IS_IMPORTANT": "N",
            "TOGGLED_BY": "503",
            "TOGGLED_DATE": "2025-11-10T15:02:33+02:00",
            "MEMBERS": [
                {
                    "ID": "11",
                    "TYPE": "U",
                    "NAME": "John Doe",
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
[`string`](../../data-types.md) | Identifier of the checklist item ||
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
- `N` — regular ||
|| **TOGGLED_BY** 
[`string`](../../data-types.md) | Identifier of the user who last changed the item's status.

Can be `null` if the status has not been changed ||
|| **TOGGLED_DATE** 
[`string`](../../data-types.md) | Date and time of the item's status change in `ISO 8601` format ||
|| **MEMBERS** 
[`array`](../../data-types.md) | A list of objects with [description of participants](#members) ||
|| **ATTACHMENTS** 
[`object`](../../data-types.md) | An object with [description of attached files](#attachments).

The key is the identifier of the attached file `ATTACHMENT_ID` ||
|#

#### Members Object {#members}

#| 
|| **Name**
`type` | **Description** ||
|| **ID** 
[`string`](../../data-types.md) | Identifier of the user ||
|| **TYPE** 
[`string`](../../data-types.md) | User's role in the checklist item. Possible values:
- `A` — Participant,
- `U` — Observer ||
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

HTTP Status: **400**

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
|| `ERROR_CORE` | TASKS_ERROR_EXCEPTION_#8; Action failed; 8\/TE\/ACTION_FAILED_TO_BE_PROCESSED\u003Cbr\u003E | User does not have access to the task ||
|| `ERROR_CORE` | TASKS_ERROR_EXCEPTION_#256; Param #0 (taskId) for method ctaskchecklistitem::getlist() expected to be of type \u0022integer\u0022, but given something else.; 256\/TE\/WRONG_ARGUMENTS\u003Cbr\u003E | Required parameter `TASKID` is missing or has an incorrect type ||
|| `ERROR_CORE` | TASKS_ERROR_EXCEPTION_#256; Param #1 (arOrder) for method ctaskchecklistitem::getlist() must not contain key \u0022IS_COMPLETED\u0022.; 256\/TE\/WRONG_ARGUMENTS\u003Cbr\u003E | An invalid field was specified in `ORDER` ||
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