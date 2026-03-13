# Move Checklist Item Before Another Item: `tasks.template.checklist.moveBefore`

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permissions to modify the task template

The method `tasks.template.checklist.moveBefore` moves a checklist item before the specified item.

## Method Parameters

{% include [Footnote on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **templateId***  
[`integer`](../../../data-types.md) | The identifier of the task template.

The task template identifier can be obtained when [creating a new template](../tasks-template-add.md) ||
|| **checkListItemId***  
[`integer`](../../../data-types.md) | The identifier of the item to be moved.

The checklist item identifier can be obtained when [creating a new item](./tasks-template-checklist-add.md) or by using the [method to get the list of items](./tasks-template-checklist-list.md) ||
|| **beforeItemId***  
[`integer`](../../../data-types.md) | The identifier of the item before which the element should be placed.

The checklist item identifier can be obtained when [creating a new item](./tasks-template-checklist-add.md) or by using the [method to get the list of items](./tasks-template-checklist-list.md) ||
|#

## Code Examples

{% include [Footnote on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
      "templateId": 139,
      "checkListItemId": 37,
      "beforeItemId": 29
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/tasks.template.checklist.moveBefore
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
      "templateId": 139,
      "checkListItemId": 37,
      "beforeItemId": 29,
      "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/tasks.template.checklist.moveBefore
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'tasks.template.checklist.moveBefore',
            {
                templateId: 139,
                checkListItemId: 37,
                beforeItemId: 29
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
                'tasks.template.checklist.moveBefore',
                [
                    'templateId' => 139,
                    'checkListItemId' => 37,
                    'beforeItemId' => 29
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
        'tasks.template.checklist.moveBefore',
        {
            templateId: 139,
            checkListItemId: 37,
            beforeItemId: 29
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
        'tasks.template.checklist.moveBefore',
        [
            'templateId' => 139,
            'checkListItemId' => 37,
            'beforeItemId' => 29
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
            "attachments": {
                "1417": "n5065",
                "1419": "n5067"
            },
            "nodeId": null,
            "templateId": 139
        }
    },
    "time": {
        "start": 1773298275,
        "finish": 1773298275.937379,
        "duration": 0.9373788833618164,
        "processing": 0,
        "date_start": "2026-03-12T09:51:15+01:00",
        "date_finish": "2026-03-12T09:51:15+01:00",
        "operating_reset_at": 1773298875,
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

#### Result Object {#result}

#|
|| **Name**
`type` | **Description** ||
|| **checkListItem**
[`object`](../../../data-types.md) | The checklist item after being moved [(detailed description)](#checklistitem) ||
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
|| `400` | `100` | Could not find value for parameter {afterItemId} | Required parameter `afterItemId` not provided ||
|| `400` | `0` | Bitrix\Tasks\CheckList\CheckListFacade::onAfterUpdate(): Argument #1 ($taskId) must be of type int, string given, called in /var/www/html/bitrix/modules/tasks/lib/checklist/checklistfacade.php on line 313 | Empty or invalid type for `templateId` provided ||
|| `400` | `0` | Invalid value [] for field [ENTITY_ID] in element [, ] | Non-existent, empty, or invalid type for `checkListItemId` provided ||
|| `400` | `0` | Moving item: action unavailable | User does not have permission to modify the task template ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./tasks-template-checklist-add.md)
- [{#T}](./tasks-template-checklist-update.md)
- [{#T}](./tasks-template-checklist-get.md)
- [{#T}](./tasks-template-checklist-list.md)
- [{#T}](./tasks-template-checklist-delete.md)
- [{#T}](./tasks-template-checklist-move-after.md)
- [{#T}](./tasks-template-checklist-complete.md)
- [{#T}](./tasks-template-checklist-renew.md)
- [{#T}](./tasks-template-checklist-add-attachment-by-content.md)
- [{#T}](./tasks-template-checklist-add-attachments-from-disk.md)
- [{#T}](./tasks-template-checklist-remove-attachments.md)