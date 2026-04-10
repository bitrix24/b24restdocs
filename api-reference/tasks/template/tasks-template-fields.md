# Get the List of Task Template Fields `tasks.template.fields`

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `tasks.template.fields` returns the description of the task template fields.

## Method Parameters

No parameters.

## Code Examples

{% include [Example Notes](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/tasks.template.fields
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
      "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/tasks.template.fields
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'tasks.template.fields',
            {}
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
                'tasks.template.fields',
                []
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        print_r($result);

    } catch (Throwable $e) {
        echo 'Error retrieving template fields: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'tasks.template.fields',
        {},
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
        'tasks.template.fields',
        []
    );

    print_r($result);
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "ID": {
            "title": "ID",
            "type": "integer",
            "primary": true
        },
        "PARENT_ID": {
            "title": "Parent Task",
            "type": "integer",
            "default": 0
        },
        "TITLE": {
            "title": "Title",
            "type": "string",
            "required": true
        },
        "DESCRIPTION": {
            "title": "Description",
            "type": "string"
        },
        "PRIORITY": {
            "title": "Priority",
            "type": "enum",
            "values": {
                "2": null,
                "1": null,
                "0": null
            },
            "default": 1
        },
        "GROUP_ID": {
            "title": "Project",
            "type": "integer",
            "default": 0
        },
        "STAGE_ID": {
            "title": "Stage",
            "type": "integer",
            "default": 0
        },
        "CREATED_BY": {
            "title": "Creator",
            "type": "integer",
            "required": true
        },
        "RESPONSIBLE_ID": {
            "title": "Responsible",
            "type": "integer",
            "required": true
        },
        "DEPENDS_ON": {
            "title": "Dependency",
            "type": "integer",
            "required": true
        },
        "RESPONSIBLES": {
            "title": "Responsible Persons",
            "type": "array"
        },
        "ACCOMPLICES": {
            "title": "Participants",
            "type": "array"
        },
        "AUDITORS": {
            "title": "Observers",
            "type": "array"
        },
        "STATUS": {
            "title": "Status",
            "type": "enum",
            "values": {
                "2": null,
                "3": null,
                "4": null,
                "5": null,
                "6": null
            },
            "default": 2
        },
        "MULTITASK": {
            "title": "Multitask",
            "type": "enum",
            "values": {
                "Y": "Yes",
                "N": "No"
            },
            "default": "N"
        },
        "REPLICATE": {
            "title": "Recurring Task",
            "type": "enum",
            "values": {
                "Y": "Yes",
                "N": "No"
            },
            "default": "N"
        },
        "SITE_ID": {
            "title": "SITE_ID",
            "type": "string"
        },
        "TASK_CONTROL": {
            "title": "Accept Work",
            "type": "enum",
            "values": {
                "Y": "Yes",
                "N": "No"
            },
            "default": "N"
        },
        "ADD_IN_REPORT": {
            "title": "Add to Report",
            "type": "enum",
            "values": {
                "Y": "Yes",
                "N": "No"
            },
            "default": "N"
        },
        "XML_ID": {
            "title": "XML_ID",
            "type": "string",
            "default": null
        },
        "DEADLINE_AFTER": {
            "title": "Deadline After",
            "type": "datetime",
            "default": null
        },
        "START_DATE_PLAN_AFTER": {
            "title": "Planned Start After",
            "type": "datetime",
            "default": null
        },
        "END_DATE_PLAN_AFTER": {
            "title": "Planned Completion After",
            "type": "datetime",
            "default": null
        },
        "TPARAM_TYPE": {
            "title": "For New Users",
            "type": "enum",
            "values": {
                "1": "Yes",
                "0": "No"
            },
            "default": 0
        },
        "TPARAM_REPLICATION_COUNT": {
            "title": "Number of Created Tasks",
            "type": "integer",
            "default": 0
        },
        "REPLICATE_PARAMS": {
            "title": "Task Creation Parameters",
            "type": "string"
        },
        "BASE_TEMPLATE_ID": {
            "title": "Parent Template",
            "type": "integer",
            "default": null
        },
        "TEMPLATE_CHILDREN_COUNT": {
            "title": "Number of Sub-templates",
            "type": "integer",
            "default": null
        },
        "UF_CRM_TASK": {
            "title": "Link to CRM Entities",
            "type": "crm"
        },
        "UF_TASK_WEBDAV_FILES": {
            "title": "File (Drive)",
            "type": "disk_file"
        },
        "UF_AUTO_291715100499": {
            "title": "String",
            "type": "string"
        },
        "UF_AUTO_152207278904": {
            "title": "Number",
            "type": "double"
        }
    },
    "time": {
        "start": 1771919518,
        "finish": 1771919518.633086,
        "duration": 0.6330859661102295,
        "processing": 0,
        "date_start": "2026-02-24T10:51:58+01:00",
        "date_finish": "2026-02-24T10:51:58+01:00",
        "operating_reset_at": 1771920118,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Title**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Object with [task template fields](./fields.md). For each field, an object with a description is returned [(detailed description)](#field-description) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Field Description Object {#field-description}

#|
|| **Title**
`type` | **Description** ||
|| **title**
[`string`](../../data-types.md) | Field name for display ||
|| **type**
[`string`](../../data-types.md) | Field value type ||
|| **required**
[`boolean`](../../data-types.md) | Indicates if the field is required. Not returned for all fields ||
|| **default**
[`string`](../../data-types.md) | Default value. Can be a string, number, or `null` ||
|| **values**
[`object`](../../data-types.md) | List of allowed values for fields of type list `enum`. The object key is the value, and the object value is its label ||
|| **primary**
[`boolean`](../../data-types.md) | Indicates if the field is primary ||
|#

## Error Handling

{% include notitle [error handling](../../../_includes/error-info.md) %}

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./tasks-template-add.md)
- [{#T}](./tasks-template-update.md)
- [{#T}](./tasks-template-get.md)
- [{#T}](./tasks-template-delete.md)