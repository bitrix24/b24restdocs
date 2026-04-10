# Get Description for Custom Fields crm.userfield.fields

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.userfield.fields` returns a description of the characteristics of custom fields.

## Method Parameters

No parameters.

## Code Examples

{% include [Footnote on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.userfield.fields
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.userfield.fields
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"crm.userfield.fields",
    		{}
    	);

    	const result = response.getData().result;
    	console.dir(result);
    }
    catch(error)
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
                'crm.userfield.fields',
                []
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        if ($result->error()) {
            error_log($result->error());
            echo 'Error: ' . $result->error();
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching CRM userfield fields: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.userfield.fields",
        {},
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.userfield.fields',
        []
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "ID": {
            "type": "int",
            "title": "ID",
            "isReadOnly": true
        },
        "ENTITY_ID": {
            "type": "string",
            "title": "Object",
            "isImmutable": true
        },
        "FIELD_NAME": {
            "type": "string",
            "title": "Code",
            "isImmutable": true
        },
        "USER_TYPE_ID": {
            "type": "string",
            "title": "Data Type",
            "isImmutable": true
        },
        "XML_ID": {
            "type": "string",
            "title": "External ID (XML ID)"
        },
        "SORT": {
            "type": "int",
            "title": "Sorting"
        },
        "MULTIPLE": {
            "type": "char",
            "title": "Multiple"
        },
        "MANDATORY": {
            "type": "char",
            "title": "Mandatory"
        },
        "SHOW_FILTER": {
            "type": "char",
            "title": "Show in list filter"
        },
        "SHOW_IN_LIST": {
            "type": "char",
            "title": "Show in list"
        },
        "EDIT_IN_LIST": {
            "type": "char",
            "title": "Allow user editing"
        },
        "IS_SEARCHABLE": {
            "type": "char",
            "title": "Field values are searchable"
        },
        "EDIT_FORM_LABEL": {
            "type": "string",
            "title": "Label in edit form"
        },
        "LIST_COLUMN_LABEL": {
            "type": "string",
            "title": "Header in list"
        },
        "LIST_FILTER_LABEL": {
            "type": "string",
            "title": "Filter label in list"
        },
        "ERROR_MESSAGE": {
            "type": "string",
            "title": "Error message"
        },
        "HELP_MESSAGE": {
            "type": "string",
            "title": "Help"
        },
        "LIST": {
            "type": "uf_enum_element",
            "title": "List elements",
            "isMultiple": true
        },
        "SETTINGS": {
            "type": "object",
            "title": "Additional settings (dependent on type)"
        }
    },
    "time": {
        "start": 1773992295,
        "finish": 1773992295.958088,
        "duration": 0.9580879211425781,
        "processing": 0,
        "date_start": "2026-03-20T10:38:15+02:00",
        "date_finish": "2026-03-20T10:38:15+02:00",
        "operating_reset_at": 1773992895,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Object with a description of characteristics [(detailed description)](#result) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Fields of the result object {#result}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../../data-types.md) | Identifier ||
|| **ENTITY_ID**
[`string`](../../data-types.md) | Custom field object ||
|| **FIELD_NAME**
[`string`](../../data-types.md) | Field code ||
|| **USER_TYPE_ID**
[`string`](../../data-types.md) | Custom field data type ||
|| **XML_ID**
[`string`](../../data-types.md) | External ID (XML ID) ||
|| **SORT**
[`integer`](../../data-types.md) | Sorting order ||
|| **MULTIPLE**
[`char`](../../data-types.md) | Multiple indicator ||
|| **MANDATORY**
[`char`](../../data-types.md) | Mandatory indicator ||
|| **SHOW_FILTER**
[`char`](../../data-types.md) | Display in list filter indicator ||
|| **SHOW_IN_LIST**
[`char`](../../data-types.md) | Display in list indicator ||
|| **EDIT_IN_LIST**
[`char`](../../data-types.md) | Allow user editing indicator ||
|| **IS_SEARCHABLE**
[`char`](../../data-types.md) | Field values are searchable indicator ||
|| **EDIT_FORM_LABEL**
[`string`](../../data-types.md)\|[`lang_map`](../../data-types.md) | Label in edit form ||
|| **LIST_COLUMN_LABEL**
[`string`](../../data-types.md)\|[`lang_map`](../../data-types.md) | Header in list ||
|| **LIST_FILTER_LABEL**
[`string`](../../data-types.md)\|[`lang_map`](../../data-types.md) | Filter label in list ||
|| **ERROR_MESSAGE**
[`string`](../../data-types.md)\|[`lang_map`](../../data-types.md) | Error message ||
|| **HELP_MESSAGE**
[`string`](../../data-types.md)\|[`lang_map`](../../data-types.md) | Help ||
|| **LIST**
[`uf_enum_element`](./crm-userfield-enumeration-fields.md) | List elements for `enumeration` type field ||
|| **SETTINGS**
[`object`](../../data-types.md) | Additional settings depending on field type ||
|#

## Error Handling

{% include notitle [error handling](../../../../_includes/error-info.md) %}

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-userfield-types.md)
- [{#T}](./crm-userfield-enumeration-fields.md)
- [{#T}](./crm-userfield-settings-fields.md)