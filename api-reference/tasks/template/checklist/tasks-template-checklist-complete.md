# Complete Checklist Item of Task Template tasks.template.checklist.complete

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: user with permissions to modify the task template

The method `tasks.template.checklist.complete` marks a checklist item of the template as completed.

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
      "checkListItemId": 27
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/tasks.template.checklist.complete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
      "templateId": 139,
      "checkListItemId": 27,
      "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/tasks.template.checklist.complete
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'tasks.template.checklist.complete',
            {
                templateId: 139,
                checkListItemId: 27
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
                'tasks.template.checklist.complete',
                [
                    'templateId' => 139,
                    'checkListItemId' => 27
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
        'tasks.template.checklist.complete',
        {
            templateId: 139,
            checkListItemId: 27
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
        'tasks.template.checklist.complete',
        [
            'templateId' => 139,
            'checkListItemId' => 27
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
            "id": 27,
            "copiedId": null,
            "userId": 503,
            "createdBy": null,
            "parentId": 23,
            "title": "2. Fill out the report form",
            "sortIndex": 1,
            "displaySortIndex": "",
            "isComplete": true,
            "isImportant": false,
            "completedCount": 0,
            "members": [],
            "attachments": [],
            "nodeId": null,
            "templateId": 139
        }
    },
    "time": {
        "start": 1773239663,
        "finish": 1773239663.724766,
        "duration": 0.7247660160064697,
        "processing": 0,
        "date_start": "2026-03-11T17:34:23+01:00",
        "date_finish": "2026-03-11T17:34:23+01:00",
        "operating_reset_at": 1773240263,
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
[`object`](../../../data-types.md) | Updated checklist item after completion [(detailed description)](#checklistitem) ||
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
|| `400` | `0` | Bitrix\Tasks\CheckList\CheckListFacade::onAfterUpdate(): Argument #1 ($taskId) must be of type int, string given, called in /var/www/html/bitrix/modules/tasks/lib/checklist/checklistfacade.php on line 313 | Empty or invalid type `templateId` provided ||
|| `400` | `0` | Invalid value [] for field [ENTITY_ID] in element [, ] | Non-existent, empty, or invalid type `checkListItemId` provided ||
|| `400` | `0` | Changing the status of the item: action unavailable | User does not have permission to modify the task template ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./tasks-template-checklist-add.md)
- [{#T}](./tasks-template-checklist-update.md)
- [{#T}](./tasks-template-checklist-get.md)
- [{#T}](./tasks-template-checklist-list.md)
- [{#T}](./tasks-template-checklist-delete.md)