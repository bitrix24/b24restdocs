# Get Custom Field Data with task.item.userfield.getfields

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `task.item.userfield.getfields` retrieves a list of fields for custom task fields.

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
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/task.item.userfield.getfields
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
      "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/task.item.userfield.getfields
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'task.item.userfield.getfields',
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
                'task.item.userfield.getfields',
                []
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
        'task.item.userfield.getfields',
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
        'task.item.userfield.getfields',
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
            "type": "int",
            "title": "ID",
            "isReadOnly": true
        },
        "ENTITY_ID": {
            "type": "string",
            "title": "Entity",
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
            "title": "Sort Order"
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
            "title": "Show in List Filter"
        },
        "SHOW_IN_LIST": {
            "type": "char",
            "title": "Show in List"
        },
        "EDIT_IN_LIST": {
            "type": "char",
            "title": "Allow User Editing"
        },
        "IS_SEARCHABLE": {
            "type": "char",
            "title": "Field Values are Searchable"
        },
        "EDIT_FORM_LABEL": {
            "type": "string",
            "title": "Edit Form Label"
        },
        "LIST_COLUMN_LABEL": {
            "type": "string",
            "title": "List Column Header"
        },
        "LIST_FILTER_LABEL": {
            "type": "string",
            "title": "List Filter Label"
        },
        "ERROR_MESSAGE": {
            "type": "string",
            "title": "Error Message"
        },
        "HELP_MESSAGE": {
            "type": "string",
            "title": "Help"
        },
        "LIST": {
            "type": "uf_enum_element",
            "title": "List Elements",
            "isMultiple": true
        },
        "SETTINGS": {
            "type": "object",
            "title": "Additional Settings (dependent on type)"
        }
    },
    "total": 0,
    "time": {
        "start": 1772710591,
        "finish": 1772710591.142614,
        "duration": 0.14261388778686523,
        "processing": 0,
        "date_start": "2026-03-05T14:36:31+01:00",
        "date_finish": "2026-03-05T14:36:31+01:00",
        "operating_reset_at": 1772711191,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Description of available custom field properties. Each key of the object contains a field description [(detailed description)](#result) ||
|| **total**
[`integer`](../../data-types.md) | Currently returns `0` ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Object result {#result}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../../data-types.md) | Identifier. Read-only ||
|| **ENTITY_ID**
[`string`](../../data-types.md) | Entity ||
|| **FIELD_NAME**
[`string`](../../data-types.md) | Code. Immutable ||
|| **USER_TYPE_ID**
[`string`](../../data-types.md) | Data Type. Immutable ||
|| **XML_ID**
[`string`](../../data-types.md) | External Identifier ||
|| **SORT**
[`integer`](../../data-types.md) | Sort Order ||
|| **MULTIPLE**
[`char`](../../data-types.md) | Multiple ||
|| **MANDATORY**
[`char`](../../data-types.md) | Mandatory ||
|| **SHOW_FILTER**
[`char`](../../data-types.md) | Show in List Filter ||
|| **SHOW_IN_LIST**
[`char`](../../data-types.md) | Show in List ||
|| **EDIT_IN_LIST**
[`char`](../../data-types.md) | Allow User Editing ||
|| **IS_SEARCHABLE**
[`char`](../../data-types.md) | Field Values are Searchable ||
|| **EDIT_FORM_LABEL**
[`string`](../../data-types.md) | Edit Form Label ||
|| **LIST_COLUMN_LABEL**
[`string`](../../data-types.md) | List Column Header ||
|| **LIST_FILTER_LABEL**
[`string`](../../data-types.md) | List Filter Label ||
|| **ERROR_MESSAGE**
[`string`](../../data-types.md) | Error Message ||
|| **HELP_MESSAGE**
[`string`](../../data-types.md) | Help ||
|| **LIST**
[`uf_enum_element`](../../data-types.md) | List Elements. Multiple ||
|| **SETTINGS**
[`object`](../../data-types.md) | Additional Settings ||
|#

#### Field Description Object {#field-description}

#|
|| **Name**
`type` | **Description** ||
|| **type**
[`string`](../../data-types.md) | Field Data Type ||
|| **title**
[`string`](../../data-types.md) | Field Name ||
|| **isReadOnly**
[`boolean`](../../data-types.md) | Read-only Field Indicator ||
|| **isImmutable**
[`boolean`](../../data-types.md) | Immutable Field Indicator ||
|| **isMultiple**
[`boolean`](../../data-types.md) | Multiple Field Indicator ||
|#

## Error Handling

{% include notitle [Error Handling](../../../_includes/error-info.md) %}

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./task-item-user-field-add.md)
- [{#T}](./task-item-user-field-update.md)
- [{#T}](./task-item-user-field-get.md)
- [{#T}](./task-item-user-field-get-list.md)
- [{#T}](./task-item-user-field-delete.md)
- [{#T}](./task-item-user-field-get-types.md)