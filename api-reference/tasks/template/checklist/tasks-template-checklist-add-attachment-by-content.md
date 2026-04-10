# Add Attachment to Checklist Item Using tasks.template.checklist.addAttachmentByContent

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permissions to modify the task template

The method `tasks.template.checklist.addAttachmentByContent` adds an attachment to a checklist item based on the file's content.

## Method Parameters

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **templateId***
[`integer`](../../../data-types.md) | Identifier of the task template.

The task template identifier can be obtained when [creating a new template](../tasks-template-add.md) ||
|| **checkListItemId***
[`integer`](../../../data-types.md) | Identifier of the checklist item.

The checklist item identifier can be obtained when [creating a new item](./tasks-template-checklist-add.md) or by using the [method to retrieve the list of items](./tasks-template-checklist-list.md) ||
|| **attachmentParameters***
[`object`](../../../data-types.md) | Parameters for the created attachment [(detailed description)](#attachmentparameters) ||
|#

### Parameter attachmentParameters {#attachmentparameters}

#|
|| **Name**
`type` | **Description** ||
|| **NAME**
[`string`](../../../data-types.md) | Name of the created file ||
|| **CONTENT**
[`string`](../../../data-types.md) | Content of the file in base64 format ||
|#

{% note tip "Typical Use-Cases and Scenarios" %}

- [How to Upload Files](../../../files/how-to-upload-files.md)

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
      "checkListItemId": 37,
      "attachmentParameters": {
        "NAME": "dashboard-note.txt",
        "CONTENT": "RGFzaGJvYXJkIG5vdGU="
      }
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/tasks.template.checklist.addAttachmentByContent
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
      "templateId": 139,
      "checkListItemId": 37,
      "attachmentParameters": {
        "NAME": "dashboard-note.txt",
        "CONTENT": "RGFzaGJvYXJkIG5vdGU="
      },
      "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/tasks.template.checklist.addAttachmentByContent
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'tasks.template.checklist.addAttachmentByContent',
            {
                templateId: 139,
                checkListItemId: 37,
                attachmentParameters: {
                    NAME: 'dashboard-note.txt',
                    CONTENT: 'RGFzaGJvYXJkIG5vdGU='
                }
            }
        );

        const result = response.getData().result;
        console.log(result);
    }
    catch (error)
    {
        console.error(error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'tasks.template.checklist.addAttachmentByContent',
                [
                    'templateId' => 139,
                    'checkListItemId' => 37,
                    'attachmentParameters' => [
                        'NAME' => 'dashboard-note.txt',
                        'CONTENT' => 'RGFzaGJvYXJkIG5vdGU='
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
        'tasks.template.checklist.addAttachmentByContent',
        {
            templateId: 139,
            checkListItemId: 37,
            attachmentParameters: {
                NAME: 'dashboard-note.txt',
                CONTENT: 'RGFzaGJvYXJkIG5vdGU='
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
        'tasks.template.checklist.addAttachmentByContent',
        [
            'templateId' => 139,
            'checkListItemId' => 37,
            'attachmentParameters' => [
                'NAME' => 'dashboard-note.txt',
                'CONTENT' => 'RGFzaGJvYXJkIG5vdGU='
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
        "start": 1773303072,
        "finish": 1773303072.349161,
        "duration": 0.34916090965270996,
        "processing": 0,
        "date_start": "2026-03-12T11:11:12+01:00",
        "date_finish": "2026-03-12T11:11:12+01:00",
        "operating_reset_at": 1773303672,
        "operating": 0.2779998779296875
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Object containing the response data [(detailed description)](#result).

{% note info "" %}

The new file does not appear in the response of the `tasks.template.checklist.addAttachmentByContent` method under `attachments`. To retrieve data about the new file, use the [tasks.template.checklist.get](./tasks-template-checklist-get.md) method.

{% endnote %}

|| 
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Object result {#result}

#|
|| **Name**
`type` | **Description** ||
|| **checkListItem**
[`object`](../../../data-types.md) | Checklist item after adding the attachment [(detailed description)](#checklistitem) ||
|#

{% include [Decoding the checkListItem Object](./_includes/checklist-item-response.md) %}

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
|| `400` | `100` | Could not find value for parameter {templateId} | Required parameter `templateId` not provided ||
|| `400` | `100` | Bitrix\Tasks\CheckList\Internals\CheckList All parameters in the constructor must have real class type | Required parameter `checkListItemId` not provided ||
|| `400` | `100` | Could not find value for parameter {attachmentParameters} | Required parameter `attachmentParameters` not provided or is empty ||
|| `400` | `0` | An error occurred while adding the attachment | Non-existent or empty `checkListItemId` specified ||
|| `400` | `0` | Modification of the item: action not available | User does not have permission to modify the task template ||
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
- [{#T}](./tasks-template-checklist-add-attachments-from-disk.md)
- [{#T}](./tasks-template-checklist-remove-attachments.md)