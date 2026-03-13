# Get Checklist Item of Task Template tasks.template.checklist.get

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: user with read access permission for the task template

The method `tasks.template.checklist.get` returns a single checklist item from the task template.

## Method Parameters

{% include [Footnote on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **templateId***
[`integer`](../../../data-types.md) | Identifier of the task template.

The identifier of the task template can be obtained when [creating a new template](../tasks-template-add.md) ||
|| **checkListItemId***
[`integer`](../../../data-types.md) | Identifier of the checklist item in the template.

The identifier of the checklist item can be obtained when [creating a new item](./tasks-template-checklist-add.md) or by using the [method to get the list of items](./tasks-template-checklist-list.md) ||
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
      "checkListItemId": 37
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/tasks.template.checklist.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
      "templateId": 139,
      "checkListItemId": 37,
      "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/tasks.template.checklist.get
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'tasks.template.checklist.get',
            {
                templateId: 139,
                checkListItemId: 37
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
                'tasks.template.checklist.get',
                [
                    'templateId' => 139,
                    'checkListItemId' => 37
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
        'tasks.template.checklist.get',
        {
            templateId: 139,
            checkListItemId: 37,
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
        'tasks.template.checklist.get',
        [
            'templateId' => 139,
            'checkListItemId' => 37
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
        "start": 1773239193,
        "finish": 1773239193.529108,
        "duration": 0.5291080474853516,
        "processing": 0,
        "date_start": "2026-03-11T17:26:33+01:00",
        "date_finish": "2026-03-11T17:26:33+01:00",
        "operating_reset_at": 1773239793,
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
[`object`](../../../data-types.md) | Data of the checklist item [(detailed description)](#checklistitem).

Contains empty field values:
- if an empty or incorrectly typed `templateId` is specified
- if a non-existent, empty, or incorrectly typed `checkListItemId` is specified ||
|#

{% include [Decoding the checkListItem object](./_includes/checklist-item-response.md) %}

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
|| `400` | `100` | Could not find value for parameter {templateId} | Required parameter `templateId` is missing ||
|| `400` | `100` | Bitrix\Tasks\CheckList\Internals\CheckList All parameters in the constructor must have real class type | Required parameter `checkListItemId` is missing ||
|| `400` | `0` | Access to the task template is denied | Insufficient rights to read the template ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./tasks-template-checklist-add.md)
- [{#T}](./tasks-template-checklist-update.md)
- [{#T}](./tasks-template-checklist-list.md)
- [{#T}](./tasks-template-checklist-delete.md)
- [{#T}](./tasks-template-checklist-complete.md)