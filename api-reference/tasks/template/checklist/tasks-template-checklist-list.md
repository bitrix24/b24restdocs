# Get the List of Checklist Items for Task Template tasks.template.checklist.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with read access permission for the task template

The method `tasks.template.checklist.list` returns a list of checklist items for the task template.

## Method Parameters

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

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
- `IS_IMPORTANT` — importance mark of the item

The `templateId` parameter is forcibly added to the filter as `TEMPLATE_ID`, even if a different value is provided in `filter` ||
|| **select** 
[`array`](../../../data-types.md) | List of fields to select.

You can specify the following fields in the selection:
- `ID` — identifier of the checklist item
- `TEMPLATE_ID` — identifier of the task template
- `PARENT_ID` — identifier of the parent item
- `TITLE` — text of the checklist item
- `SORT_INDEX` — sort index
- `IS_COMPLETE` — completion status of the item
- `IS_IMPORTANT` — importance mark of the item
- `MEMBERS` — list of participants for the item
- `ATTACHMENTS` — list of attachments for the item ||
|| **order** 
[`object`](../../../data-types.md) | Object for sorting the result in the form `{ "field": "sort value", ... }`.

You can sort by the following fields:
- `ID` — identifier of the checklist item
- `PARENT_ID` — identifier of the parent item
- `CREATED_BY` — identifier of the item author
- `TITLE` — text of the checklist item
- `SORT_INDEX` — sort index
- `IS_COMPLETE` — completion status of the item
- `IS_IMPORTANT` — importance mark of the item
- `TOGGLED_BY` — identifier of the user who last changed the item's status
- `TOGGLED_DATE` — date and time of the status change

The sorting direction can take the following values:
- `asc` — ascending
- `desc` — descending

By default, the result is sorted by `ID` in descending order ||
|#

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
      // tasks.template.checklist.list returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
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
          start: 0,
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

          // tasks.template.checklist.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
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
[`object`](../../../data-types.md) | Object where the key is the identifier of the checklist item and the value is the description of the checklist item [(detailed description)](#checklistitems).

The set of fields in the response depends on the `select` parameter ||
|#

{% include [Decoding the checkListItem Object](./_includes/checklist-item-response.md) %}

## Error Handling

HTTP Status: **400**

```json
{
    "error": "0",
    "error_description": "Access to the task template is denied"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#| 
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `0` | Access to the task template is denied | Insufficient rights to read the template ||
|| `400` | `100` | Could not find value for parameter {templateId} | Required parameter `templateId` is missing ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

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