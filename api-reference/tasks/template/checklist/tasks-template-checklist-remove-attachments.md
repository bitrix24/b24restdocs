# Remove Attachments from Checklist Item tasks.template.checklist.removeAttachments

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permissions to modify the task template

The method `tasks.template.checklist.removeAttachments` removes attachments from a checklist item in the task template.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **templateId*** 
[`integer`](../../../data-types.md) | Identifier of the task template.

The identifier of the task template can be obtained when [creating a new template](../tasks-template-add.md) ||
|| **checkListItemId*** 
[`integer`](../../../data-types.md) | Identifier of the checklist item.

The identifier of the checklist item can be obtained when [creating a new item](./tasks-template-checklist-add.md) or by using the [method to get the list of items](./tasks-template-checklist-list.md) ||
|| **attachmentsIds*** 
[`array`](../../../data-types.md) | Array of attachment identifiers to be removed ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
      "templateId": 139,
      "checkListItemId": 37,
      "attachmentsIds": [
        1119,
        1121
      ]
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/tasks.template.checklist.removeAttachments
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
      "templateId": 139,
      "checkListItemId": 37,
      "attachmentsIds": [
        1119,
        1121
      ],
      "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/tasks.template.checklist.removeAttachments
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type CheckListItemResult = {
      checkListItem: {
        id: number
        copiedId: number | null
        userId: number
        createdBy: number | null
        parentId: number
        title: string
        sortIndex: number
        displaySortIndex: string
        isComplete: boolean
        isImportant: boolean
        completedCount: number
        members: Array<{
          id: string
          type: string
          name: string
          personalPhoto: string
          personalGender: string
          image: string
          isCollaber: boolean
        }>
        attachments: unknown[]
        nodeId: number | null
        templateId: number
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<CheckListItemResult>({
        method: 'tasks.template.checklist.removeAttachments',
        params: {
          templateId: 139,
          checkListItemId: 37,
          attachmentsIds: [1119, 1121],
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Updated checklist item:', result.checkListItem)
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
      async function removeAttachments() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'tasks.template.checklist.removeAttachments',
            params: {
              templateId: 139,
              checkListItemId: 37,
              attachmentsIds: [1119, 1121],
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Updated checklist item:', result.checkListItem)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', removeAttachments)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'tasks.template.checklist.removeAttachments',
                [
                    'templateId' => 139,
                    'checkListItemId' => 37,
                    'attachmentsIds' => [1119, 1121]
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
        'tasks.template.checklist.removeAttachments',
        {
            templateId: 139,
            checkListItemId: 37,
            attachmentsIds: [1119, 1121]
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
        'tasks.template.checklist.removeAttachments',
        [
            'templateId' => 139,
            'checkListItemId' => 37,
            'attachmentsIds' => [1119, 1121]
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
            "createdBy": null,
            "parentId": 23,
            "title": "4. Prepare the dashboard and send the link",
            "sortIndex": 2,
            "displaySortIndex": "",
            "isComplete": false,
            "isImportant": true,
            "completedCount": 0,
            "members": [
                {
                    "id": "547",
                    "type": "A",
                    "name": "Anna Peterson",
                    "personalPhoto": "57129",
                    "personalGender": "",
                    "image": "https://mysite.com/b17053/resize_cache/57129/c0120a8d7c10d63c83e32398d1ec4d9e/main/137/137bfa78b877be117e75f1ac8652834a/anna.png",
                    "isCollaber": false
                }
            ],
            "attachments": [],
            "nodeId": null,
            "templateId": 139
        }
    },
    "time": {
        "start": 1773298408,
        "finish": 1773298408.806751,
        "duration": 0.806751012802124,
        "processing": 0,
        "date_start": "2026-03-12T09:53:28+01:00",
        "date_finish": "2026-03-12T09:53:28+01:00",
        "operating_reset_at": 1773299008,
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
[`object`](../../../data-types.md) | Checklist item after attachments removal [(detailed description)](#checklistitem) ||
|#

{% include [Decoding the checkListItem object](./_includes/checklist-item-response.md) %}

## Error Handling

HTTP Status: **400**

```json
{
    "error": "100",
    "error_description": "Could not find value for parameter {templateId}"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#| 
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `100` | Could not find value for parameter {templateId} | Required parameter `templateId` is missing ||
|| `400` | `100` | Bitrix\Tasks\CheckList\Internals\CheckList All parameters in the constructor must have real class type | Required parameter `checkListItemId` is missing ||
|| `400` | `100` | Could not find value for parameter {attachmentsIds} | Required parameter `attachmentsIds` is missing ||
|| `400` | `100` | Invalid value {} to match with parameter {attachmentsIds}. Should be value of type array. | Empty or invalid type for `attachmentsIds` ||
|| `400` | `0` | Invalid value [] for field [ENTITY_ID] in element [, ] | Non-existent, empty, or invalid type for `checkListItemId` ||
|| `400` | `0` | Changing item: action is not available | User does not have permission to modify the task template ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./tasks-template-checklist-add.md)
- [{#T}](./tasks-template-checklist-update.md)
- [{#T}](./tasks-template-checklist-get.md)
- [{#T}](./tasks-template-checklist-list.md)
- [{#T}](./tasks-template-checklist-delete.md)
- [{#T}](./tasks-template-checklist-move-after.md)
- [{#T}](./tasks-template-checklist-move-before.md)
- [{#T}](./tasks-template-checklist-complete.md)
- [{#T}](./tasks-template-checklist-renew.md)
- [{#T}](./tasks-template-checklist-add-attachment-by-content.md)
- [{#T}](./tasks-template-checklist-add-attachments-from-disk.md)