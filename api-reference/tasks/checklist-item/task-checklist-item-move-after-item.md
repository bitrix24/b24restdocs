# Move Checklist Item with task.checklistitem.moveafteritem

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: a user with read access to the task who is:
> - a Bitrix24 administrator
> - the task creator or their supervisor
> - the item author or their supervisor
> - the assignee or a participant, if their role allows editing checklists
> - a workgroup member with permission to edit the group's tasks

The method `task.checklistitem.moveafteritem` moves the checklist item `ITEMID` to a position after the item `AFTERITEMID`.

Both items must belong to the same task `TASKID`. The items can be in different sublists, but after the move, `ITEMID` will receive the same `PARENT_ID` as `AFTERITEMID`.

For example, to move item `453` after item `447`, pass `ITEMID = 453` and `AFTERITEMID = 447`:

```plaintext
BEFORE:                                        AFTER:
Checklist 1 (431)                             Checklist 1 (431)
├── first item (433)                          ├── first item (433)
│   ├── subitem 1 (435)                        │   ├── subitem 1 (435)
│   ├── subitem 2 (445)                        │   └── subitem 2 (445)
│   └── subitem 3 (453) ← PARENT_ID=433       ├── second item (447)
├── second item (447)                          ├── subitem 3 (453) ← PARENT_ID=431
└── third item (449)                           └── third item (449)
```

You can check permissions to modify the item using the method [task.checklistitem.isactionallowed](./task-checklist-item-is-action-allowed.md).

## Method Parameters

{% note warning "" %}

Pass parameters in the request in the order shown in the table. If the order is violated, the request returns an error or moves the wrong item.

{% endnote %}

{% include [Note on required parameters](../../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **TASKID*** 
[`integer`](../../data-types.md) | Identifier of the task.

The task identifier can be obtained when [creating a new task](../tasks-task-add.md) or using the method [get task list](../tasks-task-list.md) ||
|| **ITEMID*** 
[`integer`](../../data-types.md) | Identifier of the checklist item being moved.

The checklist item identifier can be obtained when [creating an item](./task-checklist-item-add.md) or using the method [get checklist item list](./task-checklist-item-get-list.md) ||
|| **AFTERITEMID*** 
[`integer`](../../data-types.md) | Identifier of the checklist item after which the moving item should be placed.

The checklist item identifier can be obtained when [creating an item](./task-checklist-item-add.md) or using the method [get checklist item list](./task-checklist-item-get-list.md) ||
|#

## Code Examples

{% include [Required parameters in examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASKID":13,"ITEMID":453,"AFTERITEMID":447}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/task.checklistitem.moveafteritem
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASKID":13,"ITEMID":453,"AFTERITEMID":447,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/task.checklistitem.moveafteritem
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // result is null when the item is successfully moved
    // Shape of the payload returned in result (match the "response handling" section of the page)
    type MoveChecklistItemResult = null

    try {
      const response = await $b24.actions.v2.call.make<MoveChecklistItemResult>({
        method: 'task.checklistitem.moveafteritem',
        params: {
          TASKID: 13,
          ITEMID: 453,
          AFTERITEMID: 447,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Checklist item moved successfully, result:', result)
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
      async function moveChecklistItem() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'task.checklistitem.moveafteritem',
            params: {
              TASKID: 13,
              ITEMID: 453,
              AFTERITEMID: 447,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Checklist item moved successfully, result:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', moveChecklistItem)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.task.checklistitem.moveafteritem(
            task_id=13,
            item_id=453,
            after_item_id=447,
        ).response
        result = bitrix_response.result
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'task.checklistitem.moveafteritem',
                [
                    'TASKID' => 13,
                    'ITEMID' => 453,
                    'AFTERITEMID' => 447
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error moving checklist item: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'task.checklistitem.moveafteritem',
        {
            TASKID: 13,
            ITEMID: 453,
            AFTERITEMID: 447
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
        'task.checklistitem.moveafteritem',
        [
            'TASKID' => 13,
            'ITEMID' => 453,
            'AFTERITEMID' => 447
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "task.checklistitem.moveafteritem", b24.Params{
    	"TASKID":      13,
    	"ITEMID":      453,
    	"AFTERITEMID": 447,
    })
    if err != nil {
    	return fmt.Errorf("task.checklistitem.moveafteritem: %w", err)
    }

    // On success, result is null
    fmt.Printf("%s\n", res.Result)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": null,
    "time": {
        "start": 1764597401,
        "finish": 1764597401.936492,
        "duration": 0.9364919662475586,
        "processing": 0,
        "date_start": "2025-12-01T16:56:41+01:00",
        "date_finish": "2025-12-01T16:56:41+01:00",
        "operating_reset_at": 1764598001,
        "operating": 0.29050707817077637
    }
}
```

### Returned Data

#| 
|| **Name**
`type` | **Description** ||
|| **result**
`null` | Returns `null` if the item is successfully moved ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ERROR_CORE",
    "error_description": "TASKS_ERROR_EXCEPTION_#8; Move item: action unavailable; 8/TE/ACTION_FAILED_TO_BE_PROCESSED<br>"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#| 
|| **Code** | **Description** | **Value**  ||
|| `ERROR_CORE` | TASKS_ERROR_EXCEPTION_#256; Param #2 (afterItemId) expected by method ctaskchecklistitem::moveafteritem(), but not given.; 256/TE/WRONG_ARGUMENTS<br> | A required parameter is missing. The parameter number and name in the message: `Param #0 (taskId)`, `Param #1 (itemId)`, or `Param #2 (afterItemId)` ||
|| `ERROR_CORE` | TASKS_ERROR_EXCEPTION_#256; Param #0 (taskId) for method ctaskchecklistitem::moveafteritem() expected to be of type "integer", but given something else.; 256/TE/WRONG_ARGUMENTS<br> | Incorrect value type. The parameter number and name in the message indicate which value is incorrect ||
|| `ERROR_CORE` | TASKS_ERROR_EXCEPTION_#8; Incorrect value [] specified for field [ENTITY_ID] in item [, ]; 8/TE/ACTION_FAILED_TO_BE_PROCESSED<br> | There is no item with the `ITEMID` identifier ||
|| `ERROR_CORE` | TASKS_ERROR_ASSERT_EXCEPTION<br> | The `TASKID` or `ITEMID` value is less than or equal to zero ||
|| `ERROR_CORE` | TASKS_ERROR_EXCEPTION_#8; Parent item cannot be a subitem of itself; 8/TE/ACTION_FAILED_TO_BE_PROCESSED<br> | `AFTERITEMID` is a subitem of the item being moved `ITEMID` ||
|| `ERROR_CORE` | TASKS_ERROR_EXCEPTION_#8; Move item: action unavailable; 8/TE/ACTION_FAILED_TO_BE_PROCESSED<br> | User does not have access rights to the task or lacks permissions to perform the action ||
|#

If the `AFTERITEMID` item does not exist, the server does not respond, and the request ends with a timeout and no error code. Check the item using the [task.checklistitem.get](./task-checklist-item-get.md) method before the call.

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./task-checklist-item-add.md)
- [{#T}](./task-checklist-item-update.md)
- [{#T}](./task-checklist-item-get.md)
- [{#T}](./task-checklist-item-get-list.md)
- [{#T}](./task-checklist-item-delete.md)
- [{#T}](./task-checklist-item-complete.md)
- [{#T}](./task-checklist-item-renew.md)
- [{#T}](./task-checklist-item-is-action-allowed.md)