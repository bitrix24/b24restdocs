# Update Checklist Item of Task Template tasks.template.checklist.update

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: user with permissions to modify the task template

The method `tasks.template.checklist.update` updates a checklist item of the task template.

## Method Parameters

{% include [Footnote on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **templateId***
[`integer`](../../../data-types.md) | Identifier of the task template.

The identifier of the task template can be obtained when [creating a new template](../tasks-template-add.md) ||
|| **checkListItemId***
[`integer`](../../../data-types.md) | Identifier of the checklist item.

The identifier of the checklist item can be obtained when [creating a new item](./tasks-template-checklist-add.md) or by using the method [getting the list of items](./tasks-template-checklist-list.md) ||
|| **fields***
[`object`](../../../data-types.md) | Fields to update the checklist item [(detailed description)](#fields) ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **TITLE**
[`string`](../../../data-types.md) | Text of the checklist item

If `PARENT_ID` is passed with a value of `0`, then `TITLE` is the name of the checklist ||
|| **PARENT_ID**
[`integer`](../../../data-types.md) | Identifier of the parent item. Use for nested checklists

- If `PARENT_ID` is passed with a value of `0`, the system will create a new checklist in the task template
- If there is no checklist item with the specified `PARENT_ID` in the template, the system will create a new checklist
- If the main checklist item is moved under another checklist item, it will move along with its sub-items while maintaining the hierarchy. The checklists will merge into one ||
|| **SORT_INDEX**
[`integer`](../../../data-types.md) | Sort index. The lower the value, the higher the item in the list or sublist ||
|| **IS_COMPLETE**
[`boolean`](../../../data-types.md) | Completion status of the item. Possible values:
- `Y` — completed
- `N` — not completed ||
|| **IS_IMPORTANT**
[`boolean`](../../../data-types.md) | Mark indicating that the item is important. Possible values:
- `Y` — important
- `N` — ordinary ||
|| **MEMBERS**
[`object`](../../../data-types.md) | Object describing the participants of the checklist item. Key — user identifier, value — object with the participant type parameter `TYPE`. Possible participant type values:
- `'TYPE': 'A'` — Participant
- `'TYPE': 'U'` — Observer

The `MEMBERS` field is completely replaced. To retain current participants, pass them along with new values

The system will add checklist item participants to the task in the same roles ||
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
      "fields": {
        "TITLE": "4. Prepare the dashboard and send the link"
      }
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/tasks.template.checklist.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
      "templateId": 139,
      "checkListItemId": 37,
      "fields": {
        "TITLE": "4. Prepare the dashboard and send the link"
      },
      "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/tasks.template.checklist.update
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'tasks.template.checklist.update',
            {
                templateId: 139,
                checkListItemId: 37,
                fields: {
                    TITLE: '4. Prepare the dashboard and send the link'
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
                'tasks.template.checklist.update',
                [
                    'templateId' => 139,
                    'checkListItemId' => 37,
                    'fields' => [
                        'TITLE' => '4. Prepare the dashboard and send the link'
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
        'tasks.template.checklist.update',
        {
            templateId: 139,
            checkListItemId: 37,
            fields: {
                TITLE: '4. Prepare the dashboard and send the link'
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
        'tasks.template.checklist.update',
        [
            'templateId' => 139,
            'checkListItemId' => 37,
            'fields' => [
                'TITLE' => '4. Prepare the dashboard and send the link'
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
            "attachments": [],
            "nodeId": null,
            "templateId": 139
        }
    },
    "time": {
        "start": 1773238088,
        "finish": 1773238088.917228,
        "duration": 0.9172279834747314,
        "processing": 0,
        "date_start": "2026-03-11T17:08:08+01:00",
        "date_finish": "2026-03-11T17:08:08+01:00",
        "operating_reset_at": 1773238688,
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
[`object`](../../../data-types.md) | Updated checklist item [(detailed description)](#checklistitem) ||
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
|| `400` | `100` | Could not find value for parameter {fields} | Required parameter `fields` not provided or empty ||
|| `400` | `0` | Bitrix\Tasks\CheckList\CheckListFacade::onAfterUpdate(): Argument #1 ($taskId) must be of type int, string given, called in /var/www/html/bitrix/modules/tasks/lib/checklist/checklistfacade.php on line 313 | Empty or incorrect type `templateId` provided ||
|| `400` | `0` | Incorrect value [] for field [ENTITY_ID] in item [, Item name] | Non-existent, empty, or incorrect type `checkListItemId` provided ||
|| `400` | `0` | Changing item: action not available | User does not have permission to modify the task template ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./tasks-template-checklist-add.md)
- [{#T}](./tasks-template-checklist-get.md)
- [{#T}](./tasks-template-checklist-list.md)
- [{#T}](./tasks-template-checklist-delete.md)
- [{#T}](./tasks-template-checklist-complete.md)