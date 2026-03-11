# Get User Field of Task by ID task.item.userfield.get

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `task.item.userfield.get` retrieves the description of a user field of a task by its ID.

## Method Parameters

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ID***
[`integer`](../../data-types.md) | Identifier of the user field.

The identifier of the task's user field can be obtained when [creating the field](./task-item-user-field-add.md) or by using the [method to get the list of fields](./task-item-user-field-get-list.md) ||
|#

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
      "ID": 1325
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/task.item.userfield.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
      "ID": 1325,
      "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/task.item.userfield.get
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'task.item.userfield.get',
            {
                ID: 1325
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
                'task.item.userfield.get',
                [
                    'ID' => 1325
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
        'task.item.userfield.get',
        {
            ID: 1325
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
        'task.item.userfield.get',
        [
            'ID' => 1325
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
        },
        "EDIT_FORM_LABEL": {
            "ar": "",
            "br": "",
            "de": "",
            "en": "Description of client request",
            ...
        },
        "LIST_COLUMN_LABEL": {
            "en": "Сlient request",
            ...
        },
        "LIST_FILTER_LABEL": {
            "en": "Сlient request",
            ...
        },
        "ERROR_MESSAGE": {
            "en": "Сlient request",
            ...
        },
        "HELP_MESSAGE": {
            "en": "Сlient request",
            ...
        }
    },
    "total": 0,
    "time": {
        "start": 1772718119,
        "finish": 1772718119.677154,
        "duration": 0.6771540641784668,
        "processing": 0,
        "date_start": "2026-03-05T16:41:59+01:00",
        "date_finish": "2026-03-05T16:41:59+01:00",
        "operating_reset_at": 1772718719,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | User field object [(detailed description)](#result) ||
|| **total**
[`integer`](../../data-types.md) | Currently returns `0` ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

#### Result Object {#result}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../../data-types.md) | Identifier of the user field ||
|| **ENTITY_ID**
[`string`](../../data-types.md) | Code of the object to which the field is linked ||
|| **FIELD_NAME**
[`string`](../../data-types.md) | Code of the user field ||
|| **USER_TYPE_ID**
[`string`](../../data-types.md) | Data type ||
|| **XML_ID**
[`string`](../../data-types.md) | External identifier ||
|| **SORT**
[`integer`](../../data-types.md) | Sort order ||
|| **MULTIPLE**
[`char`](../../data-types.md) | Indicates multiple values. Possible values:
- `Y` — multiple
- `N` — single ||
|| **MANDATORY**
[`char`](../../data-types.md) | Indicates required field. Possible values:
- `Y` — required
- `N` — not required ||
|| **SHOW_FILTER**
[`char`](../../data-types.md) | Display in the list filter ||
|| **SHOW_IN_LIST**
[`char`](../../data-types.md) | Display in the list ||
|| **EDIT_IN_LIST**
[`char`](../../data-types.md) | Editing allowed in the list ||
|| **IS_SEARCHABLE**
[`char`](../../data-types.md) | Value is searchable ||
|| **EDIT_FORM_LABEL**
[`object`](../../data-types.md) | Localized labels in the edit form ||
|| **LIST_COLUMN_LABEL**
[`object`](../../data-types.md) | Localized column headers in the list ||
|| **LIST_FILTER_LABEL**
[`object`](../../data-types.md) | Localized labels in the list filter ||
|| **ERROR_MESSAGE**
[`object`](../../data-types.md) | Localized validation error texts ||
|| **HELP_MESSAGE**
[`object`](../../data-types.md) | Localized hints for the field ||
|| **SETTINGS**
[`object`](../../data-types.md) | Additional field settings [(detailed description)](#settings) ||
|#

#### SETTINGS Object {#settings}

#|
|| **Name**
`type` | **Description** ||
|| **SIZE**
[`integer`](../../data-types.md) | Input field width ||
|| **ROWS**
[`integer`](../../data-types.md) | Number of rows in the input field ||
|| **REGEXP**
[`string`](../../data-types.md) | Regular expression for value validation ||
|| **MIN_LENGTH**
[`integer`](../../data-types.md) | Minimum length of the value ||
|| **MAX_LENGTH**
[`integer`](../../data-types.md) | Maximum length of the value ||
|| **DEFAULT_VALUE**
[`string`](../../data-types.md) | Default value ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ERROR_CORE",
    "error_description": "ID is not defined or invalid."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `ERROR_CORE` | No parameters found | Required parameter `ID` not provided ||
|| `400` | `ERROR_CORE` | ID is not defined or invalid | Non-numeric value or value `<= 0` provided for parameter `ID` ||
|| `400` | `ERROR_NOT_FOUND` | The entity with ID '{ID}' is not found | User field with the specified `ID` not found ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./task-item-user-field-add.md)
- [{#T}](./task-item-user-field-update.md)
- [{#T}](./task-item-user-field-get-list.md)
- [{#T}](./task-item-user-field-delete.md)
- [{#T}](./task-item-user-field-get-types.md)
- [{#T}](./task-item-user-field-get-fields.md)