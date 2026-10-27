# Get the List of Checklist Items for Task Template tasks.template.checklist.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with read access permission for the task template

The method `tasks.template.checklist.list` returns a list of checklist items for the task template.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#| 
|| **Name** 
`type` | **Description** ||
|| **templateId*** 
[`integer`](../../../data-types.md) | Identifier of the task template.

The identifier of the task template can be obtained when [creating a new template](../tasks-template-add.md) ||
|| **filter** 
[`object`](../../../data-types.md) | Filter for selecting checklist items in the form `{ "field": "value", ... }`.

You can filter by the following fields:
- `ID` — identifier of the checklist item
- `TEMPLATE_ID` — identifier of the task template
- `PARENT_ID` — identifier of the parent item
- `TITLE` — text of the checklist item
- `SORT_INDEX` — sort index
- `IS_COMPLETE` — completion status of the item
- `IS_IMPORTANT` — importance mark of the item: `Y` — important, `N` — ordinary

You can pass the status and importance as `Y` and `N` or as `true` and `false`. The method skips fields not in this list without returning an error.

The `templateId` parameter is forcibly added to the filter as `TEMPLATE_ID`, even if a different value is provided in `filter` ||
|| **select** 
[`array`](../../../data-types.md) | List of fields to select. If the parameter is not passed, the method returns all fields.

You can specify the following fields in the selection:
- `ID` — identifier of the checklist item
- `TEMPLATE_ID` — identifier of the task template
- `PARENT_ID` — identifier of the parent item
- `TITLE` — text of the checklist item
- `SORT_INDEX` — sort index
- `IS_COMPLETE` — completion status of the item
- `IS_IMPORTANT` — importance mark of the item
- `MEMBERS` — list of participants for the item
- `ATTACHMENTS` — list of attachments for the item

If `select` does not include `ID`, the identifier remains only as the item key in `checkListItems` ||
|| **order** 
[`object`](../../../data-types.md) | Object for sorting the result in the form `{ "field": "sort value", ... }`.

You can sort by the following fields:
- `ID` — identifier of the checklist item
- `TEMPLATE_ID` — identifier of the task template
- `PARENT_ID` — identifier of the parent item
- `TITLE` — text of the checklist item
- `SORT_INDEX` — sort index
- `IS_COMPLETE` — completion status of the item
- `IS_IMPORTANT` — importance mark of the item

The sorting direction can take the following values:
- `asc` — ascending
- `desc` — descending

The method skips fields not in this list without returning an error.

By default, the result is sorted by `TEMPLATE_ID` in descending order, then by `SORT_INDEX` in ascending order and by `ID` in descending order ||
|#

{% note info "" %}

The method returns all matching items in a single response. It has no pagination and ignores the `start` parameter.

{% endnote %}

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
      "templateId": 139,
      "filter": {
        "IS_COMPLETE": "N",
        "IS_IMPORTANT": "N"
      },
      "select": [
        "ID",
        "TEMPLATE_ID",
        "PARENT_ID",
        "TITLE",
        "SORT_INDEX",
        "IS_COMPLETE",
        "IS_IMPORTANT"
      ],
      "order": {
        "SORT_INDEX": "asc",
        "ID": "desc"
      }
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/tasks.template.checklist.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
      "templateId": 139,
      "filter": {
        "IS_COMPLETE": "N",
        "IS_IMPORTANT": "N"
      },
      "select": [
        "ID",
        "TEMPLATE_ID",
        "PARENT_ID",
        "TITLE",
        "SORT_INDEX",
        "IS_COMPLETE",
        "IS_IMPORTANT"
      ],
      "order": {
        "SORT_INDEX": "asc",
        "ID": "desc"
      },
      "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/tasks.template.checklist.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type CheckListItem = {
      id: string
      templateId: string
      parentId: number | string
      title: string
      sortIndex: string
      isComplete: string
      isImportant: string
    }

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type CheckListItemsResult = {
      checkListItems: Record<string, CheckListItem>
    }

    try {
      // tasks.template.checklist.list returns all matching items in one response:
      // the method has no pagination and ignores `start`, so a single call.make is enough
      const response = await $b24.actions.v2.call.make<CheckListItemsResult>({
        method: 'tasks.template.checklist.list',
        params: {
          templateId: 139,
          filter: {
            IS_COMPLETE: 'N',
            IS_IMPORTANT: 'N',
          },
          select: [
            'ID',
            'TEMPLATE_ID',
            'PARENT_ID',
            'TITLE',
            'SORT_INDEX',
            'IS_COMPLETE',
            'IS_IMPORTANT',
          ],
          order: {
            SORT_INDEX: 'asc',
            ID: 'desc',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(
          'Checklist items count:', Object.keys(result.checkListItems).length,
          'Items:', result.checkListItems
        )
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
      async function listTemplateChecklistItems() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // tasks.template.checklist.list returns all matching items in one response:
          // the method has no pagination and ignores `start`, so a single call.make is enough
          const response = await $b24.actions.v2.call.make({
            method: 'tasks.template.checklist.list',
            params: {
              templateId: 139,
              filter: {
                IS_COMPLETE: 'N',
                IS_IMPORTANT: 'N',
              },
              select: [
                'ID',
                'TEMPLATE_ID',
                'PARENT_ID',
                'TITLE',
                'SORT_INDEX',
                'IS_COMPLETE',
                'IS_IMPORTANT',
              ],
              order: {
                SORT_INDEX: 'asc',
                ID: 'desc',
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
          console.info(
            'Checklist items count:', Object.keys(result.checkListItems).length,
            'Items:', result.checkListItems
          )
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listTemplateChecklistItems)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.tasks.template.checklist.list(
            template_id=139,
            filter={
                "IS_COMPLETE": "N",
                "IS_IMPORTANT": "N",
            },
            select=[
                "ID",
                "TEMPLATE_ID",
                "PARENT_ID",
                "TITLE",
                "SORT_INDEX",
                "IS_COMPLETE",
                "IS_IMPORTANT",
            ],
            order={
                "SORT_INDEX": "asc",
                "ID": "desc",
            },
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
                'tasks.template.checklist.list',
                [
                    'templateId' => 139,
                    'filter' => [
                        'IS_COMPLETE' => 'N',
                        'IS_IMPORTANT' => 'N',
                    ],
                    'select' => [
                        'ID',
                        'TEMPLATE_ID',
                        'PARENT_ID',
                        'TITLE',
                        'SORT_INDEX',
                        'IS_COMPLETE',
                        'IS_IMPORTANT',
                    ],
                    'order' => [
                        'SORT_INDEX' => 'asc',
                        'ID' => 'desc',
                    ]
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        print_r($result);
    } catch (Throwable $e) {
        echo $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'tasks.template.checklist.list',
        {
            templateId: 139,
            filter: {
                IS_COMPLETE: 'N',
                IS_IMPORTANT: 'N'
            },
            select: [
                'ID',
                'TEMPLATE_ID',
                'PARENT_ID',
                'TITLE',
                'SORT_INDEX',
                'IS_COMPLETE',
                'IS_IMPORTANT'
            ],
            order: {
                SORT_INDEX: 'asc',
                ID: 'desc'
            }
        },
        function(result)
        {
            if (result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'tasks.template.checklist.list',
        [
            'templateId' => 139,
            'filter' => [
                'IS_COMPLETE' => 'N',
                'IS_IMPORTANT' => 'N',
            ],
            'select' => [
                'ID',
                'TEMPLATE_ID',
                'PARENT_ID',
                'TITLE',
                'SORT_INDEX',
                'IS_COMPLETE',
                'IS_IMPORTANT',
            ],
            'order' => [
                'SORT_INDEX' => 'asc',
                'ID' => 'desc',
            ]
        ]
    );

    print_r($result);
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "tasks.template.checklist.list", b24.Params{
    	"templateId": 139,
    	"filter": b24.Params{
    		"IS_COMPLETE":  "N",
    		"IS_IMPORTANT": "N",
    	},
    	"select": []string{"ID", "TEMPLATE_ID", "PARENT_ID", "TITLE", "SORT_INDEX", "IS_COMPLETE", "IS_IMPORTANT"},
    	"order": b24.Params{
    		"SORT_INDEX": "asc",
    		"ID":         "desc",
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("tasks.template.checklist.list: %w", err)
    }

    // The method wraps the response in an object with the "checkListItems" key.
    raw, ok := b24.Unwrap(res.Result, "checkListItems")
    if !ok {
    	return fmt.Errorf("no checkListItems key in the response")
    }

    fmt.Printf("%s\n", raw)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "checkListItems": {
            "23": {
                "id": "23",
                "templateId": "139",
                "parentId": 0,
                "title": "What needs to be done",
                "sortIndex": "0",
                "isComplete": "N",
                "isImportant": "N"
            },
            "27": {
                "id": "27",
                "templateId": "139",
                "parentId": "23",
                "title": "2. Fill out the report form",
                "sortIndex": "1",
                "isComplete": "N",
                "isImportant": "N"
            },
            "29": {
                "id": "29",
                "templateId": "139",
                "parentId": "23",
                "title": "3. Send the report to the manager",
                "sortIndex": "3",
                "isComplete": "N",
                "isImportant": "N"
            },
            "25": {
                "id": "25",
                "templateId": "139",
                "parentId": "23",
                "title": "1. Gather data",
                "sortIndex": "4",
                "isComplete": "N",
                "isImportant": "N"
            }
        }
    },
    "time": {
        "start": 1773316805,
        "finish": 1773316806.016895,
        "duration": 1.016895055770874,
        "processing": 0,
        "date_start": "2026-03-12T15:00:05+01:00",
        "date_finish": "2026-03-12T15:00:06+01:00",
        "operating_reset_at": 1773317406,
        "operating": 0
    }
}
```

### Returned Data

#| 
|| **Name** 
`type` | **Description** ||
|| **result** 
[`object`](../../../data-types.md) | Object containing the response data [(detailed description)](#result) ||
|| **time** 
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Object result {#result}

#| 
|| **Name** 
`type` | **Description** ||
|| **checkListItems** 
[`object`](../../../data-types.md) | Object where the key is the identifier of the checklist item and the value is the description of the item [(detailed description)](#checklistitem).

Items follow the sort order from `order`. In JavaScript, an object with numeric keys is iterated in ascending key order, so to display items in the required order, sort them by `sortIndex` and `parentId`.

If there are no items, an empty array `[]` is returned ||
|#

#### Object checkListItem {#checklistitem}

The set of fields depends on the `select` parameter: the response includes only the fields listed in it. If `select` is not passed, all fields from the table are returned.

The method returns values as strings, except for the `parentId` of a root item, which is returned as the number `0`. This distinguishes the response from the [tasks.template.checklist.get](./tasks-template-checklist-get.md) and [tasks.template.checklist.add](./tasks-template-checklist-add.md) methods, where numbers and flags are typed.

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../../data-types.md) | Identifier of the checklist item ||
|| **templateId**
[`string`](../../../data-types.md) | Identifier of the task template ||
|| **parentId**
[`string`](../../../data-types.md) \| [`integer`](../../../data-types.md) | Identifier of the parent item. For a root item, the number `0` ||
|| **title**
[`string`](../../../data-types.md) | Text of the checklist item ||
|| **sortIndex**
[`string`](../../../data-types.md) | Sorting index among items at the same level. The lower the value, the higher the item ||
|| **isComplete**
[`string`](../../../data-types.md) | Completion status of the item:
- `Y` — completed
- `N` — not completed ||
|| **isImportant**
[`string`](../../../data-types.md) | Importance flag of the item:
- `Y` — important
- `N` — regular ||
|| **members**
[`array`](../../../data-types.md) | Array of item participants [(detailed description)](#members).

If there are no participants, an empty array `[]` is returned ||
|| **attachments**
[`object`](../../../data-types.md) | Object where the key is the attachment identifier and the value is the file description [(detailed description)](#attachments).

If there are no attachments, an empty array `[]` is returned ||
|#

#### Object members {#members}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../../data-types.md) | Identifier of the user ||
|| **type**
[`string`](../../../data-types.md) | Role of the user in the checklist item:
- `A` — Participant
- `U` — Observer ||
|| **name**
[`string`](../../../data-types.md) | User's first and last name ||
|| **personalPhoto**
[`string`](../../../data-types.md) | Identifier of the user's avatar file ||
|| **personalGender**
[`string`](../../../data-types.md) | Gender of the user:
- `M` — Male
- `F` — Female
- empty string — not specified ||
|| **image**
[`string`](../../../data-types.md) | Link to the user's avatar ||
|| **isCollaber**
[`boolean`](../../../data-types.md) | Indicator that the user is an external participant from a collab ||
|#

#### Object attachments {#attachments}

Example value:

```json
"attachments": {
    "621": {
        "attachmentId": 621,
        "name": "report.txt",
        "size": "28",
        "fileId": "10921",
        "downloadUrl": "/bitrix/tools/disk/uf.php?attachedId=621&action=download&ncc=1",
        "viewUrl": "/bitrix/tools/disk/uf.php?attachedId=621&action=show&ncc=1"
    }
}
```

#|
|| **Name**
`type` | **Description** ||
|| **attachmentId**
[`integer`](../../../data-types.md) | Attachment identifier. Pass it in the `attachmentsIds` parameter of the [tasks.template.checklist.removeAttachments](./tasks-template-checklist-remove-attachments.md) method to delete the attachment ||
|| **name**
[`string`](../../../data-types.md) | File name ||
|| **size**
[`string`](../../../data-types.md) | File size in bytes ||
|| **fileId**
[`string`](../../../data-types.md) | Identifier of the file on Drive ||
|| **downloadUrl**
[`string`](../../../data-types.md) | Relative link to download the file ||
|| **viewUrl**
[`string`](../../../data-types.md) | Relative link to view the file ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "0",
    "error_description": "Access to task template was denied"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#| 
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `100` | Could not find value for parameter {templateId} | Required parameter `templateId` is missing ||
|| `400` | `100` | Invalid value {abc} to match with parameter {templateId}. Should be value of type int. | `templateId` contains a value that cannot be converted to a number ||
|| `400` | `0` | Access to task template was denied | Insufficient rights to read the template ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./tasks-template-checklist-add.md)
- [{#T}](./tasks-template-checklist-update.md)
- [{#T}](./tasks-template-checklist-get.md)
- [{#T}](./tasks-template-checklist-delete.md)
- [{#T}](./tasks-template-checklist-move-after.md)
- [{#T}](./tasks-template-checklist-move-before.md)
- [{#T}](./tasks-template-checklist-complete.md)
- [{#T}](./tasks-template-checklist-renew.md)
- [{#T}](./tasks-template-checklist-add-attachment-by-content.md)
- [{#T}](./tasks-template-checklist-add-attachments-from-disk.md)
- [{#T}](./tasks-template-checklist-remove-attachments.md)