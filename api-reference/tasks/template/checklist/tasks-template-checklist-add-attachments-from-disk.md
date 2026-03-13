# Add Attachments from Drive to Checklist Item tasks.template.checklist.addAttachmentsFromDisk

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permissions to modify the task template

The method `tasks.template.checklist.addAttachmentsFromDisk` adds files from Drive to a checklist item.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **templateId***
[`integer`](../../../data-types.md) | Identifier of the task template.

The task template identifier can be obtained when [creating a new template](../tasks-template-add.md) ||
|| **checkListItemId***
[`integer`](../../../data-types.md) | Identifier of the checklist item.

The checklist item identifier can be obtained when [creating a new item](./tasks-template-checklist-add.md) or by using the [method to get the list of items](./tasks-template-checklist-list.md) ||
|| **filesIds***
[`array`](../../../data-types.md) | An array of file identifiers from Drive. Prefix each identifier with `n`, for example, `["n5065", "n5067"]`.

File identifiers can be obtained in two ways.

Use one of the file upload methods:
  - [disk.storage.uploadfile](../../../disk/storage/disk-storage-upload-file.md)
  - [disk.folder.uploadfile](../../../disk/folder/disk-folder-upload-file.md)

Use one of the methods to get the list of files:
  - [disk.storage.getchildren](../../../disk/storage/disk-storage-get-children.md)
  - [disk.folder.getchildren](../../../disk/folder/disk-folder-get-children.md) ||
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
      "filesIds": [
        "n5065",
        "n5067"
      ]
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/tasks.template.checklist.addAttachmentsFromDisk
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
      "templateId": 139,
      "checkListItemId": 37,
      "filesIds": [
        "n5065",
        "n5067"
      ],
      "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/tasks.template.checklist.addAttachmentsFromDisk
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'tasks.template.checklist.addAttachmentsFromDisk',
            {
                templateId: 139,
                checkListItemId: 37,
                filesIds: ['n5065', 'n5067']
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
                'tasks.template.checklist.addAttachmentsFromDisk',
                [
                    'templateId' => 139,
                    'checkListItemId' => 37,
                    'filesIds' => ['n5065', 'n5067']
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
        'tasks.template.checklist.addAttachmentsFromDisk',
        {
            templateId: 139,
            checkListItemId: 37,
            filesIds: ['n5065', 'n5067']
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
        'tasks.template.checklist.addAttachmentsFromDisk',
        [
            'templateId' => 139,
            'checkListItemId' => 37,
            'filesIds' => ['n5065', 'n5067']
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
            "sortIndex": 3,
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
            "attachments": {
                "1417": "n5065",
                "1419": "n5067"
            },
            "nodeId": null,
            "templateId": 139
        }
    },
    "time": {
        "start": 1773238932,
        "finish": 1773238932.539085,
        "duration": 0.5390849113464355,
        "processing": 0,
        "date_start": "2026-03-11T17:22:12+01:00",
        "date_finish": "2026-03-11T17:22:12+01:00",
        "operating_reset_at": 1773239532,
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
|| **checkListItem**
[`object`](../../../data-types.md) | Checklist item after attachments have been added [(detailed description)](#checklistitem) ||
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
|| `400` | `100` | Could not find value for parameter {templateId} | Required parameter `templateId` not provided ||
|| `400` | `100` | Bitrix\Tasks\CheckList\Internals\CheckList All parameters in the constructor must have real class type | Required parameter `checkListItemId` not provided ||
|| `400` | `100` | Could not find value for parameter {filesIds} | Required parameter `filesIds` not provided ||
|| `400` | `100` | Invalid value {} to match with parameter {filesIds}. Should be value of type array. | Empty or invalid type for `filesIds` provided ||
|| `400` | `0` | Invalid value [] for field [ENTITY_ID] in item [, ] | Non-existent, empty, or invalid type for `checkListItemId` provided ||
|| `400` | `0` | Changing item: action not available | User does not have permission to modify the task template ||
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
- [{#T}](./tasks-template-checklist-remove-attachments.md)