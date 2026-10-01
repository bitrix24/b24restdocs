# Add Checklist Item with task.checklistitem.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: a user with read access to the task who is:
> - a Bitrix24 administrator
> - the assignee or a participant, if their role allows adding checklist items
> - the task creator or their supervisor
> - a workgroup member with permission to edit the group's tasks

The method `task.checklistitem.add` adds a new checklist item to a task.

You can check permissions for adding an item using the method [task.checklistitem.isactionallowed](./task-checklist-item-is-action-allowed.md).

## Method Parameters

{% note warning "" %}

Pass parameters in the request in the order shown in the table. If the order is violated, the request returns an error.

{% endnote %}

{% include [Note on required parameters](../../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **TASKID*** 
[`integer`](../../data-types.md) | Task identifier.

The task identifier can be obtained when [creating a new task](../tasks-task-add.md) or using the [get task list method](../tasks-task-list.md) ||
|| **FIELDS*** 
[`object`](../../data-types.md) | Object with [checklist item fields](#fields) ||
|#

### FIELDS Parameter {#fields}

{% include [Note on required parameters](../../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **TITLE*** 
[`string`](../../data-types.md) | Text of the checklist item.

If `PARENT_ID` is passed with a value of `0`, then `TITLE` is the name of the checklist ||
|| **SORT_INDEX** 
[`integer`](../../data-types.md) | Sort index. The lower the value, the higher the item in the list or sublist ||
|| **IS_COMPLETE** 
[`string`](../../data-types.md) | Status of the item. Possible values:
- `Y` — completed
- `N` — not completed

Also accepts `true` and `false`. Default is `N`.

If you create an item as already completed, the `TOGGLED_BY` and `TOGGLED_DATE` fields remain empty ||
|| **IS_IMPORTANT** 
[`string`](../../data-types.md) | Mark indicating that the item is important. Possible values:
- `Y` — important
- `N` — normal

Also accepts `true` and `false`. Default is `N` ||
|| **MEMBERS** 
[`object`](../../data-types.md) | Object describing the participants of the checklist item. Key — user identifier, value — object with the participant type parameter `TYPE`. Possible participant type values:
- `'TYPE': 'A'` — Participant
- `'TYPE': 'U'` — Observer

The system will add checklist item participants to the task in the same roles ||
|| **PARENT_ID** 
[`integer`](../../data-types.md) | Identifier of the parent item. Use for nested checklists.

- If `PARENT_ID` is passed with a value of `0`, the system will create a new checklist in the task
- If there is no item with the specified `PARENT_ID`, the item is saved with this `PARENT_ID` and does not belong to any checklist. Pass only identifiers of existing items in the task
- If `PARENT_ID` is not specified in `FIELDS`, the system will add a new item to the existing top-level checklist. If there is no checklist in the task, it will create a new one ||
|#

## Code Examples

{% include [Example Note](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASKID":13,"FIELDS":{"TITLE":"Prepare the report","PARENT_ID":457,"SORT_INDEX":200,"IS_COMPLETE":"N","IS_IMPORTANT":"Y","MEMBERS":{"547":{"TYPE":"A"}}}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/task.checklistitem.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASKID":13,"FIELDS":{"TITLE":"Prepare the report","PARENT_ID":457,"SORT_INDEX":200,"IS_COMPLETE":"N","IS_IMPORTANT":"Y","MEMBERS":{"547":{"TYPE":"A"}}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/task.checklistitem.add
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type ChecklistItemAddResult = number

    try {
      const response = await $b24.actions.v2.call.make<ChecklistItemAddResult>({
        method: 'task.checklistitem.add',
        params: {
          TASKID: 13,
          FIELDS: {
            TITLE: 'Prepare the report',
            PARENT_ID: 457,
            SORT_INDEX: 200,
            IS_COMPLETE: 'N',
            IS_IMPORTANT: 'Y',
            MEMBERS: {
              547: {
                TYPE: 'A',
              },
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
        console.info('Created checklist item with ID:', result)
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
      async function addChecklistItem() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'task.checklistitem.add',
            params: {
              TASKID: 13,
              FIELDS: {
                TITLE: 'Prepare the report',
                PARENT_ID: 457,
                SORT_INDEX: 200,
                IS_COMPLETE: 'N',
                IS_IMPORTANT: 'Y',
                MEMBERS: {
                  547: {
                    TYPE: 'A',
                  },
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
          console.info('Created checklist item with ID:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addChecklistItem)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    fields = {
        "TITLE": "Prepare the report",
        "PARENT_ID": 457,
        "SORT_INDEX": 200,
        "IS_COMPLETE": "N",
        "IS_IMPORTANT": "Y",
        "MEMBERS": {
            "547": {
                "TYPE": "A",
            },
        },
    }

    try:
        bitrix_response = client.task.checklistitem.add(
            task_id=13,
            fields=fields,
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
                'task.checklistitem.add',
                [
                    'TASKID' => 13,
                    'FIELDS' => [
                        'TITLE' => 'Prepare the report',
                        'PARENT_ID' => 457,
                        'SORT_INDEX' => 200,
                        'IS_COMPLETE' => 'N',
                        'IS_IMPORTANT' => 'Y',
                        'MEMBERS' => [
                            547 => [
                                'TYPE' => 'A'
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
        echo 'Error adding checklist item: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'task.checklistitem.add',
        {
            'TASKID': 13,
            'FIELDS': {
                'TITLE': 'Prepare the report',
                'PARENT_ID': 457,
                'SORT_INDEX': 200,
                'IS_COMPLETE': 'N',
                'IS_IMPORTANT': 'Y',
                'MEMBERS': {
                    547: {
                        'TYPE': 'A'
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
        'task.checklistitem.add',
        [
            'TASKID' => 13,
            'FIELDS' => [
                'TITLE' => 'Prepare the report',
                'PARENT_ID' => 457,
                'SORT_INDEX' => 200,
                'IS_COMPLETE' => 'N',
                'IS_IMPORTANT' => 'Y',
                'MEMBERS' => [
                    547 => [
                        'TYPE' => 'A'
                    ]
                ]
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "task.checklistitem.add", b24.Params{
    	"TASKID": 13,
    	"FIELDS": b24.Params{
    		"TITLE":        "Prepare the report",
    		"PARENT_ID":    457,
    		"SORT_INDEX":   200,
    		"IS_COMPLETE":  "N",
    		"IS_IMPORTANT": "Y",
    		"MEMBERS": b24.Params{
    			"547": b24.Params{
    				"TYPE": "A",
    			},
    		},
    	},
    })
    if err != nil {
    	return fmt.Errorf("task.checklistitem.add: %w", err)
    }

    var newID b24.ID
    if err := json.Unmarshal(res.Result, &newID); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("id:", newID)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": 475,
    "time": {
        "start": 1762431907,
        "finish": 1762431908.259832,
        "duration": 1.2598319053649902,
        "processing": 0,
        "date_start": "2025-11-06T15:25:07+01:00",
        "date_finish": "2025-11-06T15:25:08+01:00",
        "operating_reset_at": 1762432508,
        "operating": 0.24803590774536133
    }
}
```

### Returned Data

#| 
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../../data-types.md) | Identifier of the new checklist item ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error":"ERROR_CORE",
    "error_description":"TASKS_ERROR_EXCEPTION_#8; Add item: action unavailable; 8/TE/ACTION_FAILED_TO_BE_PROCESSED<br>"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#| 
|| **Code** | **Description** | **Value**  ||
|| `ERROR_CORE` | TASKS_ERROR_EXCEPTION_#8; Add item: action unavailable; 8/TE/ACTION_FAILED_TO_BE_PROCESSED<br> | No access to the task or insufficient permissions to work with checklists in the task ||
|| `ERROR_CORE` | TASKS_ERROR_EXCEPTION_#256; Param #0 (taskId) expected by method ctaskchecklistitem::add(), but not given.; 256/TE/WRONG_ARGUMENTS<br> | Required parameter `TASKID` not provided ||
|| `ERROR_CORE` | TASKS_ERROR_ASSERT_EXCEPTION<br> | The `TASKID` value is less than or equal to zero ||
|| `ERROR_CORE` | TASKS_ERROR_EXCEPTION_#256; Param #0 (taskId) for method ctaskchecklistitem::add() expected to be of type "integer", but given something else.; 256/TE/WRONG_ARGUMENTS<br> | Incorrect type for `TASKID`, or the parameters are passed out of order ||
|| `ERROR_CORE` | TASKS_ERROR_EXCEPTION_#256; Param #1 (arFields) expected by method ctaskchecklistitem::add(), but not given.; 256/TE/WRONG_ARGUMENTS<br> | Required parameter `FIELDS` not provided ||
|| `ERROR_CORE` | TASKS_ERROR_EXCEPTION_#8; Item name is missing; 8/TE/ACTION_FAILED_TO_BE_PROCESSED<br> | `FIELDS` has no `TITLE` field, or `FIELDS` is empty ||
|| `ERROR_CORE` | TASKS_ERROR_EXCEPTION_#8; Incorrect value [] specified for field [TITLE] in item [, ]; 8/TE/ACTION_FAILED_TO_BE_PROCESSED<br> | An empty string is passed in `TITLE` ||
|| `ERROR_CORE` | TASKS_ERROR_EXCEPTION_#256; Param #1 (arFields) for method ctaskchecklistitem::add() must not contain key "FOO".; 256/TE/WRONG_ARGUMENTS<br> | `FIELDS` contains a field that is not listed in the [FIELDS parameter](#fields) table ||
|| `ERROR_CORE` | TASKS_ERROR_EXCEPTION_#4; Cannot edit the task due to insufficient permissions; 4/TE/ACTION_NOT_ALLOWED<br> | Participants are passed in `MEMBERS`, but the user lacks permission to edit the task to add them to it. The item has already been created ||
|| `ERROR_CORE` | TASKS_ERROR_EXCEPTION_#8; Unknown user type passed [X]; 8/TE/ACTION_FAILED_TO_BE_PROCESSED<br> | `MEMBERS` specifies a participant type other than `A` and `U` ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./task-checklist-item-update.md)
- [{#T}](./task-checklist-item-get.md)
- [{#T}](./task-checklist-item-get-list.md)
- [{#T}](./task-checklist-item-delete.md)
- [{#T}](./task-checklist-item-move-after-item.md)
- [{#T}](./task-checklist-item-complete.md)
- [{#T}](./task-checklist-item-renew.md)
- [{#T}](./task-checklist-item-is-action-allowed.md)