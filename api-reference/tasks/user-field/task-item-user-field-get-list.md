# Get a List of Custom Fields task.item.userfield.getlist

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `task.item.userfield.getlist` retrieves a list of custom fields for tasks.

The list will include three system fields for linking to other objects:

- `UF_CRM_TASK` — with CRM objects
- `UF_MAIL_MESSAGE` — with the e-mail
- `UF_TASK_WEBDAV_FILES` — with files from Drive

These fields are created based on custom fields, which is why they appear in the list. For more information about the relationships of tasks with other objects, refer to the article [Tasks: Overview of Methods](../index.md).

## Method Parameters

{% include [Footnote on Required Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ORDER**
[`object`](../../data-types.md) | Object for sorting the result in the format `{"field": "sort value", ... }`.

You can sort by the following fields:
- `ID` — identifier of the custom field
- `FIELD_NAME` — code of the custom field
- `USER_TYPE_ID` — type of the custom field
- `SORT` — sort value

The sort direction can take the following values:
- `asc` — ascending
- `desc` — descending ||
|| **FILTER**
[`object`](../../data-types.md) | Object for filtering the result in the format `{"field": "filter value", ... }`. The value of the filtered field can be a single value or an array of values.

You can filter by the following fields:
- `ID` — identifier of the custom field
- `FIELD_NAME` — code of the custom field
- `USER_TYPE_ID` — type of the custom field
- `XML_ID` — external identifier
- `MULTIPLE` — multiple, `Y` or `N`
- `MANDATORY` — mandatory, `Y` or `N`
||
|#

## Code Examples

{% include [Footnote on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
      "ORDER": {
        "SORT": "ASC"
      }
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/task.item.userfield.getlist
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
      "ORDER": {
        "SORT": "ASC"
      },
      "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/task.item.userfield.getlist
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'task.item.userfield.getlist',
            {
                ORDER: {
                    SORT: 'ASC'
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
                'task.item.userfield.getlist',
                [
                    'ORDER' => [
                        'SORT' => 'ASC'
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
        'task.item.userfield.getlist',
        {
            ORDER: {
                SORT: 'ASC'
            },
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
        'task.item.userfield.getlist',
        [
            'ORDER' => [
                'SORT' => 'ASC'
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
    "result": [
        {
            "ID": "1295",
            "ENTITY_ID": "TASKS_TASK",
            "FIELD_NAME": "UF_CRM_TASK",
            "USER_TYPE_ID": "crm",
            "XML_ID": "",
            "SORT": "100",
            "MULTIPLE": "Y",
            "MANDATORY": "N",
            "SHOW_FILTER": "N",
            "SHOW_IN_LIST": "N",
            "EDIT_IN_LIST": "N",
            "IS_SEARCHABLE": "N",
            "SETTINGS": {
                "LEAD": "Y",
                "CONTACT": "Y",
                "COMPANY": "Y",
                "DEAL": "Y"
            }
        },
        {
            "ID": "662",
            "ENTITY_ID": "TASKS_TASK",
            "FIELD_NAME": "UF_MAIL_MESSAGE",
            "USER_TYPE_ID": "mail_message",
            "XML_ID": "",
            "SORT": "100",
            "MULTIPLE": "N",
            "MANDATORY": "N",
            "SHOW_FILTER": "N",
            "SHOW_IN_LIST": "N",
            "EDIT_IN_LIST": "N",
            "IS_SEARCHABLE": "N",
            "SETTINGS": []
        },
        {
            "ID": "229",
            "ENTITY_ID": "TASKS_TASK",
            "FIELD_NAME": "UF_TASK_WEBDAV_FILES",
            "USER_TYPE_ID": "disk_file",
            "XML_ID": "TASK_WEBDAV_FILES",
            "SORT": "100",
            "MULTIPLE": "Y",
            "MANDATORY": "N",
            "SHOW_FILTER": "N",
            "SHOW_IN_LIST": "Y",
            "EDIT_IN_LIST": "Y",
            "IS_SEARCHABLE": "Y",
            "SETTINGS": {
                "IBLOCK_ID": 0,
                "SECTION_ID": 0,
                "UF_TO_SAVE_ALLOW_EDIT": ""
            }
        },
        {
            "ID": "1325",
            "ENTITY_ID": "TASKS_TASK",
            "FIELD_NAME": "UF_TASK_CLIENT_REQUEST",
            "USER_TYPE_ID": "string",
            "XML_ID": "UF_TASK_CLIENT_REQUEST",
            "SORT": "220",
            "MULTIPLE": "N",
            "MANDATORY": "N",
            "SHOW_FILTER": "N",
            "SHOW_IN_LIST": "Y",
            "EDIT_IN_LIST": "Y",
            "IS_SEARCHABLE": "N",
            "SETTINGS": {
                "SIZE": 20,
                "ROWS": 10,
                "REGEXP": "",
                "MIN_LENGTH": 0,
                "MAX_LENGTH": 0,
                "DEFAULT_VALUE": "Clarify the goal and expected outcome"
            }
        },
        {
            "ID": "1333",
            "ENTITY_ID": "TASKS_TASK",
            "FIELD_NAME": "UF_TASK_PROJECT_BUDGET",
            "USER_TYPE_ID": "double",
            "XML_ID": "UF_TASK_PROJECT_BUDGET",
            "SORT": "230",
            "MULTIPLE": "N",
            "MANDATORY": "N",
            "SHOW_FILTER": "N",
            "SHOW_IN_LIST": "Y",
            "EDIT_IN_LIST": "Y",
            "IS_SEARCHABLE": "N",
            "SETTINGS": {
                "PRECISION": 0,
                "SIZE": 20,
                "MIN_VALUE": 0,
                "MAX_VALUE": 0,
                "DEFAULT_VALUE": 0
            }
        },
        {
            "ID": "1335",
            "ENTITY_ID": "TASKS_TASK",
            "FIELD_NAME": "UF_TASK_APPROVAL_DEADLINE",
            "USER_TYPE_ID": "datetime",
            "XML_ID": "UF_TASK_APPROVAL_DEADLINE",
            "SORT": "240",
            "MULTIPLE": "N",
            "MANDATORY": "N",
            "SHOW_FILTER": "N",
            "SHOW_IN_LIST": "Y",
            "EDIT_IN_LIST": "Y",
            "IS_SEARCHABLE": "N",
            "SETTINGS": {
                "DEFAULT_VALUE": {
                    "TYPE": "NONE",
                    "VALUE": ""
                },
                "USE_SECOND": "Y",
                "USE_TIMEZONE": "N"
            }
        },
        {
            "ID": "1337",
            "ENTITY_ID": "TASKS_TASK",
            "FIELD_NAME": "UF_TASK_LEGAL_REVIEW_REQUIRED",
            "USER_TYPE_ID": "boolean",
            "XML_ID": "UF_TASK_LEGAL_REVIEW_REQUIRED",
            "SORT": "250",
            "MULTIPLE": "N",
            "MANDATORY": "N",
            "SHOW_FILTER": "N",
            "SHOW_IN_LIST": "Y",
            "EDIT_IN_LIST": "Y",
            "IS_SEARCHABLE": "N",
            "SETTINGS": {
                "DEFAULT_VALUE": 0,
                "DISPLAY": "CHECKBOX",
                "LABEL": ["", ""],
                "LABEL_CHECKBOX": ""
            }
        }
    ],
    "total": 0,
    "time": {
        "start": 1772718556,
        "finish": 1772718556.080225,
        "duration": 0.08022499084472656,
        "processing": 0,
        "date_start": "2026-03-05T16:49:16+01:00",
        "date_finish": "2026-03-05T16:49:16+01:00",
        "operating_reset_at": 1772719156,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | Array of custom field objects [(detailed description)](#result) ||
|| **total**
[`integer`](../../data-types.md) | Currently returns `0` ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

#### Object result {#result}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../../data-types.md) | Identifier of the custom field ||
|| **ENTITY_ID**
[`string`](../../data-types.md) | Code of the object to which the field is linked ||
|| **FIELD_NAME**
[`string`](../../data-types.md) | Code of the custom field ||
|| **USER_TYPE_ID**
[`string`](../../data-types.md) | Data type ||
|| **XML_ID**
[`string`](../../data-types.md) | External identifier ||
|| **SORT**
[`integer`](../../data-types.md) | Sort value ||
|| **MULTIPLE**
[`char`](../../data-types.md) | Indicates multiple values. Possible values:
- `Y` — multiple
- `N` — single ||
|| **MANDATORY**
[`char`](../../data-types.md) | Indicates mandatory field. Possible values:
- `Y` — mandatory
- `N` — optional ||
|| **SHOW_FILTER**
[`char`](../../data-types.md) | Display in the list filter ||
|| **SHOW_IN_LIST**
[`char`](../../data-types.md) | Display in the list ||
|| **EDIT_IN_LIST**
[`char`](../../data-types.md) | Editing allowed in the list ||
|| **IS_SEARCHABLE**
[`char`](../../data-types.md) | Value is searchable ||
|| **SETTINGS**
[`object`](../../data-types.md) | Additional settings for the field, composition depends on the type `USER_TYPE_ID` ||
|#

## Error Handling

{% include notitle [Error Handling](../../../_includes/error-info.md) %}

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./task-item-user-field-add.md)
- [{#T}](./task-item-user-field-update.md)
- [{#T}](./task-item-user-field-get.md)
- [{#T}](./task-item-user-field-delete.md)
- [{#T}](./task-item-user-field-get-types.md)
- [{#T}](./task-item-user-field-get-fields.md)