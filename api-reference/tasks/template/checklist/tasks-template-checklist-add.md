# Add Checklist Item to Task Template tasks.template.checklist.add

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permissions to modify the task template

The method `tasks.template.checklist.add` adds a checklist item to the task template.

## Method Parameters

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **templateId*** 
[`integer`](../../../data-types.md) | Identifier of the task template.

The identifier of the task template can be obtained when [creating a new template](../tasks-template-add.md) ||
|| **fields*** 
[`object`](../../../data-types.md) | Fields of the created checklist item [(detailed description)](#fields) ||
|#

### Parameter fields {#fields}

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **TITLE*** 
[`string`](../../../data-types.md) | Text of the checklist item

If `PARENT_ID` is passed with a value of `0`, then `TITLE` is the name of the checklist ||
|| **PARENT_ID*** 
[`integer`](../../../data-types.md) | Identifier of the parent item.

To create a new checklist, pass `PARENT_ID` with a value of `0`.

If there is no checklist item in the template with the specified `PARENT_ID`, the system will create a new checklist ||
|| **SORT_INDEX** 
[`integer`](../../../data-types.md) | Sort index. The lower the value, the higher the item in the list or sublist ||
|| **IS_COMPLETE** 
[`boolean`](../../../data-types.md) | Status of the item. Possible values:
- `Y` — completed
- `N` — not completed

Default is `N` ||
|| **IS_IMPORTANT** 
[`boolean`](../../../data-types.md) | Mark indicating that the item is important. Possible values:
- `Y` — important
- `N` — ordinary ||
|| **MEMBERS** 
[`object`](../../../data-types.md) | Object describing the participants of the checklist item. Key — user identifier, value — object with the participant type parameter `TYPE`. Possible participant type values:
- `'TYPE': 'A'` — Participant
- `'TYPE': 'U'` — Observer

The system will add the participants of the checklist item to the task in the same roles ||
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
      "fields": {
        "TITLE": "4. Prepare the dashboard",
        "PARENT_ID": 23,
        "SORT_INDEX": 200,
        "IS_COMPLETE": "N",
        "IS_IMPORTANT": "Y",
        "MEMBERS": {
          "547": {
            "TYPE": "A"
          }
        }
      }
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/tasks.template.checklist.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
      "templateId": 139,
      "fields": {
        "TITLE": "4. Prepare the dashboard",
        "PARENT_ID": 23,
        "SORT_INDEX": 200,
        "IS_COMPLETE": "N",
        "IS_IMPORTANT": "Y",
        "MEMBERS": {
          "547": {
            "TYPE": "A"
          }
        }
      },
      "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/tasks.template.checklist.add
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type ChecklistItemResult = {
      checkListItem: {
        id: number
        copiedId: number | null
        userId: number
        createdBy: number
        parentId: number
        title: string
        sortIndex: number
        displaySortIndex: string
        isComplete: boolean
        isImportant: boolean
        completedCount: number
        members: { type: string; id: number }[]
        attachments: unknown[]
        nodeId: number | null
        templateId: number
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<ChecklistItemResult>({
        method: 'tasks.template.checklist.add',
        params: {
          templateId: 139,
          fields: {
            TITLE: '4. Prepare the dashboard',
            PARENT_ID: 23,
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
        console.info(result.checkListItem.id, result.checkListItem.title, result.checkListItem.isComplete)
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
            method: 'tasks.template.checklist.add',
            params: {
              templateId: 139,
              fields: {
                TITLE: '4. Prepare the dashboard',
                PARENT_ID: 23,
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
          console.info(result.checkListItem.id, result.checkListItem.title, result.checkListItem.isComplete)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addChecklistItem)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'tasks.template.checklist.add',
                [
                    'templateId' => 139,
                    'fields' => [
                        'TITLE' => '4. Prepare the dashboard',
                        'PARENT_ID' => 23,
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

        print_r($result);
    } catch (Throwable $e) {
        echo $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'tasks.template.checklist.add',
        {
            templateId: 139,
            fields: {
                TITLE: '4. Prepare the dashboard',
                PARENT_ID: 23,
                SORT_INDEX: 200,
                IS_COMPLETE: 'N',
                IS_IMPORTANT: 'Y',
                MEMBERS: {
                    547: {
                        TYPE: 'A'
                    }
                }
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
        'tasks.template.checklist.add',
        [
            'templateId' => 139,
            'fields' => [
                'TITLE' => '4. Prepare the dashboard',
                'PARENT_ID' => 23,
                'SORT_INDEX' => 200,
                'IS_COMPLETE' => 'N',
                'IS_IMPORTANT' => 'Y',
                'MEMBERS' => [
                    547 => [
                        'TYPE' => 'A'
                    ]
                ],
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
        "checkListItem": {
            "id": 37,
            "copiedId": null,
            "userId": 503,
            "createdBy": 503,
            "parentId": 23,
            "title": "4. Prepare the dashboard",
            "sortIndex": 200,
            "displaySortIndex": "",
            "isComplete": false,
            "isImportant": true,
            "completedCount": 0,
            "members": [
                {
                    "type": "A",
                    "id": 547
                }
            ],
            "attachments": [],
            "nodeId": null,
            "templateId": 139
        }
    },
    "time": {
        "start": 1773235651,
        "finish": 1773235651.960878,
        "duration": 0.9608778953552246,
        "processing": 0,
        "date_start": "2026-03-11T16:27:31+01:00",
        "date_finish": "2026-03-11T16:27:31+01:00",
        "operating_reset_at": 1773236251,
        "operating": 0
    }
}
```

### Returned Data

#| 
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Object with response data [(detailed description)](#result) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Object result {#result}

#| 
|| **Name**
`type` | **Description** ||
|| **checkListItem**
[`object`](../../../data-types.md) | Created checklist item [(detailed description)](#checklistitem) ||
|#

{% include [Decoding the checkListItem Object](./_includes/checklist-item-response.md) %}

## Error Handling

HTTP Status: **400**

```json
{
    "error": "0",
    "error_description": "Invalid value [] for field [PARENT_ID] in element [, New item]"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#| 
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `100` | Could not find value for parameter {templateId} | Required parameter `templateId` is missing ||
|| `400` | `100` | Could not find value for parameter {fields} | Required parameter `fields` is missing ||
|| `400` | `0` | Item name is not specified | Required parameter `fields.TITLE` is missing or has an invalid value ||
|| `400` | `0` | Invalid value [] for field [PARENT_ID] in element [, Item name] | Required parameter `fields.PARENT_ID` is missing or has an invalid value ||
|| `400` | `0` | Adding item: action is not available | User does not have permission to add a checklist item to the task template ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./tasks-template-checklist-update.md)
- [{#T}](./tasks-template-checklist-get.md)
- [{#T}](./tasks-template-checklist-list.md)
- [{#T}](./tasks-template-checklist-delete.md)
- [{#T}](./tasks-template-checklist-complete.md)