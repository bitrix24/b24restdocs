# Update Checklist Item with task.checklistitem.update

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method:
> - any user with edit access to the task
> - Creator, Performer, and Participants of the task

The method `task.checklistitem.update` modifies an existing checklist item.

You can check the permissions to modify the item using the method [task.checklistitem.isactionallowed](./task-checklist-item-is-action-allowed.md).

## Method Parameters

{% note warning "" %}

Pass parameters in the request according to the order in the table. If the order is violated, the request will return an error.

{% endnote %}

{% include [Parameter Note](../../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **TASKID*** 
[`integer`](../../data-types.md) | Task identifier.

The task identifier can be obtained when [creating a new task](../tasks-task-add.md) or using the [get task list](../tasks-task-list.md) method. ||
|| **ITEMID*** 
[`integer`](../../data-types.md) | Checklist item identifier.

The item identifier can be obtained when [adding a new item](./task-checklist-item-add.md) or using the [get checklist item list](./task-checklist-item-get-list.md) method. ||
|| **FIELDS*** 
[`object`](../../data-types.md) | Object with [checklist item fields](#fields) ||
|#

### FIELDS Parameter {#fields}

#| 
|| **Name**
`type` | **Description** ||
|| **TITLE** 
[`string`](../../data-types.md) | Text of the checklist item.

If `PARENT_ID` is passed with a value of `0`, then `TITLE` is the name of the checklist. ||
|| **SORT_INDEX** 
[`integer`](../../data-types.md) | Sort index. The lower the value, the higher the item in the list or sublist. ||
|| **IS_COMPLETE** 
[`boolean`](../../data-types.md) | Status of the item. Possible values:
- `Y` — completed
- `N` — not completed

Default is `N`. ||
|| **IS_IMPORTANT** 
[`boolean`](../../data-types.md) | Mark indicating that the item is important. Possible values:
- `Y` — important
- `N` — regular. ||
|| **MEMBERS** 
[`object`](../../data-types.md) | Object describing the participants of the checklist item. Key — user identifier, value — object with the participant type parameter `TYPE`. Possible participant type values:
- `'TYPE': 'A'` — participant
- `'TYPE': 'U'` — observer

The `MEMBERS` field is completely replaced. To retain current participants, pass them along with new values.

The system will add checklist item participants to the task in the same roles. ||
|| **PARENT_ID** 
[`integer`](../../data-types.md) | Identifier of the parent item. Use for nested checklists.

- If `PARENT_ID` is passed with a value of `0`, the system will create a new checklist in the task.
- If there is no checklist item in the task with the specified `PARENT_ID`, the system will create a new checklist.
- If the main checklist item is moved under another checklist item, it will move along with its sub-items while maintaining the hierarchy. The checklists will merge into one. ||
|#

## Code Examples

{% include [Example Note](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASKID":13,"ITEMID":475,"FIELDS":{"TITLE":"Prepare report","PARENT_ID":447,"SORT_INDEX":100,"IS_COMPLETE":"N","IS_IMPORTANT":"N","MEMBERS":{"547":{"TYPE":"A"},"125":{"TYPE":"U"}}}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/task.checklistitem.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASKID":13,"ITEMID":475,"FIELDS":{"TITLE":"Prepare report","PARENT_ID":447,"SORT_INDEX":100,"IS_COMPLETE":"N","IS_IMPORTANT":"N","MEMBERS":{"547":{"TYPE":"A"},"125":{"TYPE":"U"}}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/task.checklistitem.update
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type ChecklistItemUpdateResult = null

    try {
      const response = await $b24.actions.v2.call.make<ChecklistItemUpdateResult>({
        method: 'task.checklistitem.update',
        params: {
          TASKID: 13,
          ITEMID: 475,
          FIELDS: {
            TITLE: 'Prepare report',
            PARENT_ID: 447,
            SORT_INDEX: 100,
            IS_COMPLETE: 'N',
            IS_IMPORTANT: 'N',
            MEMBERS: {
              547: { TYPE: 'A' },
              125: { TYPE: 'U' },
            },
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Checklist item updated successfully, result:', result)
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
      async function updateChecklistItem() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'task.checklistitem.update',
            params: {
              TASKID: 13,
              ITEMID: 475,
              FIELDS: {
                TITLE: 'Prepare report',
                PARENT_ID: 447,
                SORT_INDEX: 100,
                IS_COMPLETE: 'N',
                IS_IMPORTANT: 'N',
                MEMBERS: {
                  547: { TYPE: 'A' },
                  125: { TYPE: 'U' },
                },
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Checklist item updated successfully, result:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateChecklistItem)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'task.checklistitem.update',
                [
                    'TASKID' => 13,
                    'ITEMID' => 475,
                    'FIELDS' => [
                        'TITLE' => 'Prepare report',
                        'PARENT_ID' => 447,
                        'SORT_INDEX' => 100,
                        'IS_COMPLETE' => 'N',
                        'IS_IMPORTANT' => 'N',
                        'MEMBERS' => [
                            547 => [
                                'TYPE' => 'A'
                            ],
                            125 => [
                                'TYPE' => 'U'
                            ]
                        ]
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
        echo 'Error updating checklist item: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'task.checklistitem.update',
        {
            'TASKID': 13,
            'ITEMID': 475,
            'FIELDS': {
                'TITLE': 'Prepare report',
                'PARENT_ID': 447,
                'SORT_INDEX': 100,
                'IS_COMPLETE': 'N',
                'IS_IMPORTANT': 'N',
                'MEMBERS': {
                    547: {
                        'TYPE': 'A'
                    },
                    125: {
                        'TYPE': 'U'
                    }
                }
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
        'task.checklistitem.update',
        [
            'TASKID' => 13,
            'ITEMID' => 475,
            'FIELDS' => [
                'TITLE' => 'Prepare report',
                'PARENT_ID' => 447,
                'SORT_INDEX' => 100,
                'IS_COMPLETE' => 'N',
                'IS_IMPORTANT' => 'N',
                'MEMBERS' => [
                    547 => [
                        'TYPE' => 'A'
                    ],
                    125 => [
                        'TYPE' => 'U'
                    ]
                ]
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
    "result": null,
    "time": {
        "start": 1762432505,
        "finish": 1762432505.206889,
        "duration": 0.20688891410827637,
        "processing": 0,
        "date_start": "2025-11-06T15:35:05+01:00",
        "date_finish": "2025-11-06T15:35:05+01:00",
        "operating_reset_at": 1762433105,
        "operating": 0.13953208923339844
    }
}
```

### Returned Data

#| 
|| **Name**
`type` | **Description** ||
|| **result** 
`null` | Returns `null` if the checklist item is successfully updated. ||
|| **time** 
[`time`](../../data-types.md#time) | Information about the request execution time. ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error":"ERROR_CORE",
    "error_description":"TASKS_ERROR_EXCEPTION_#4; No access to edit the task; 4\/TE\/ACTION_NOT_ALLOWED\u003Cbr\u003E"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#| 
|| **Code** | **Description** | **Value**  ||
|| `ERROR_CORE` | TASKS_ERROR_EXCEPTION_#4; No access to edit the task; 4\/TE\/ACTION_NOT_ALLOWED\u003Cbr\u003E | No permission to edit the task to modify the checklist item. ||
|| `ERROR_CORE` | TASKS_ERROR_EXCEPTION_#8; Incorrect value [] specified for field [ENTITY_ID] in item [, Prepare report]; 8\/TE\/ACTION_FAILED_TO_BE_PROCESSED\u003Cbr\u003E | Parameter order violation. ||
|| `ERROR_CORE` | "TASKS_ERROR_EXCEPTION_#256; Param #1 (itemId) for method ctaskchecklistitem::update() expected to be of type \u0022integer\u0022, but given something else.; 256\/TE\/WRONG_ARGUMENTS\u003Cbr\u003E | Required parameter `TASKID` not provided or incorrect type for `TASKID`. ||
|| `ERROR_CORE` | "TASKS_ERROR_EXCEPTION_#256; Param #1 (itemId) expected by method ctaskchecklistitem::update(), but not given.; 256\/TE\/WRONG_ARGUMENTS\u003Cbr\u003E | Required parameter `ITEMID` not provided or incorrect type for `ITEMID`. ||
|| `ERROR_CORE` | TASKS_ERROR_EXCEPTION_#256; Param #2 (arFields) expected by method ctaskchecklistitem::update(), but not given.; 256\/TE\/WRONG_ARGUMENTS\u003Cbr\u003E | Required parameter `FIELDS` not provided or empty. ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./task-checklist-item-add.md)
- [{#T}](./task-checklist-item-get.md)
- [{#T}](./task-checklist-item-get-list.md)
- [{#T}](./task-checklist-item-delete.md)
- [{#T}](./task-checklist-item-move-after-item.md)
- [{#T}](./task-checklist-item-complete.md)
- [{#T}](./task-checklist-item-renew.md)
- [{#T}](./task-checklist-item-is-action-allowed.md)