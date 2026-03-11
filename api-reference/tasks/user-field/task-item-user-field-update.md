# Update Custom Field task.item.userfield.update

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `task.item.userfield.update` updates the parameters of a task's custom field.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ID***
[`integer`](../../data-types.md) | Identifier of the custom field.

The identifier of the task's custom field can be obtained when [creating the field](./task-item-user-field-add.md) or by using the [method to get the list of fields](./task-item-user-field-get-list.md) ||
|| **DATA***
[`object`](../../data-types.md) | Set of parameters to be updated for the field [(detailed description)](#data) ||
|#

### DATA Parameter {#data}

#|
|| **Name**
`type` | **Description** ||
|| **XML_ID**
[`string`](../../data-types.md) | External identifier ||
|| **EDIT_FORM_LABEL**
[`object`](../../data-types.md) | Label in the edit form [(detailed description)](#edit_form_label) ||
|| **LABEL**
[`string`](../../data-types.md) | Name of the custom field ||
|| **SORT**
[`integer`](../../data-types.md) | Sorting
||
|| **MULTIPLE**
[`string`](../../data-types.md) | Multiple value. Possible values:
- `Y` — multiple
- `N` — single

Applicable for types `string`, `double`, `datetime`. For the `boolean` type, `N` is always used ||
|| **MANDATORY**
[`string`](../../data-types.md) | Mandatory value. Possible values:
- `Y` — mandatory
- `N` — optional
||
|| **SETTINGS**
[`object`](../../data-types.md) | Additional settings for the field type [(detailed description)](#settings) ||
|#

### EDIT_FORM_LABEL Parameter {#edit_form_label}

#|
|| **Name**
`type` | **Description** ||
|| **en**
[`string`](../../data-types.md) | Label in English ||
|#

### SETTINGS Parameter {#settings}

The fields of the `SETTINGS` object depend on the `USER_TYPE_ID` type.

{% list tabs %}

- string

    #|
    || **Name**
    `type` | **Description** ||
    || **DEFAULT_VALUE**
    [`string`](../../data-types.md) | Default value ||
    || **ROWS**
    [`integer`](../../data-types.md) | Number of rows in the input field ||
    |#

- double

    #|
    || **Name**
    `type` | **Description** ||
    || **DEFAULT_VALUE**
    [`double`](../../data-types.md) | Default value ||
    |#

- datetime

    #|
    || **Name**
    `type` | **Description** ||
    || **DEFAULT_VALUE**
     | Default value. Described as an object with two parameters:
    1. `TYPE` [`string`](../../data-types.md) — mode for setting the default value
        - `NONE` — no default value set
        - `FIXED` — uses the value from `VALUE`
        - `NOW` — uses the current time
    2. `VALUE` [`datetime`](../../data-types.md) — value for the `FIXED` type
    
    ```js
    DEFAULT_VALUE: {
        TYPE: 'NOW',
        VALUE: ''
    },
    ```
    
    ||
    |#

- boolean

    #|
    || **Name**
    `type` | **Description** ||
    || **DEFAULT_VALUE**
    [`integer`](../../data-types.md) | Default value:
    - `0` — no
    - `1` — yes ||
    || **DISPLAY**
    [`string`](../../data-types.md) | Display option for the value:
    - `CHECKBOX` — checkbox
    - `RADIO` — radio buttons
    - `DROPDOWN` — dropdown list ||
    |#

{% endlist %}

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
      "ID": 1325,
      "DATA": {
        "EDIT_FORM_LABEL": {
          "en": "Description of client request"
        },
        "MANDATORY": "N"
      }
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/task.item.userfield.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
      "ID": 1325,
      "DATA": {
        "EDIT_FORM_LABEL": {
          "en": "Description of client request"
        },
        "MANDATORY": "N"
      },
      "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/task.item.userfield.update
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'task.item.userfield.update',
            {
                ID: 1325,
                DATA: {
                    EDIT_FORM_LABEL: {
                        en: 'Description of client request'
                    },
                    MANDATORY: 'N'
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
                'task.item.userfield.update',
                [
                    'ID' => 1325,
                    'DATA' => [
                        'EDIT_FORM_LABEL' => [
                            'en' => 'Description of client request'
                        ],
                        'MANDATORY' => 'N',
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
        'task.item.userfield.update',
        {
            ID: 1325,
            DATA: {
                EDIT_FORM_LABEL: {
                    en: 'Description of client request'
                },
                MANDATORY: 'N'
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
        'task.item.userfield.update',
        [
            'ID' => 1325,
            'DATA' => [
                'EDIT_FORM_LABEL' => [
                    'en' => 'Description of client request'
                ],
                'MANDATORY' => 'N',
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
    "result": true,
    "time": {
        "start": 1740000000.000000,
        "finish": 1740000000.100000,
        "duration": 0.100000,
        "processing": 0.080000,
        "date_start": "2025-02-20T10:00:00+01:00",
        "date_finish": "2025-02-20T10:00:00+01:00",
        "operating_reset_at": 1740003600,
        "operating": 0.080000
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Returns `true` if the field was successfully updated ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
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
|| `400` | `ERROR_CORE` | TASKS_ERROR_EXCEPTION_#0; Invalid arguments for Bitrix\Tasks\Integration\Rest\Task\UserField::update; 0/TE | Required parameters `ID` and `DATA` are not provided ||
|| `400` | `ERROR_CORE` | ID is not defined or invalid | A non-numeric value or a value `<= 0` was passed to the `ID` parameter ||
|| `400` | `ERROR_NOT_FOUND` | The entity with ID '{ID}' is not found | The custom field with the specified `ID` was not found ||
|| `400` | `ERROR_CORE` | Access denied | Insufficient permissions to modify the custom field ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./task-item-user-field-add.md)
- [{#T}](./task-item-user-field-get.md)
- [{#T}](./task-item-user-field-get-list.md)
- [{#T}](./task-item-user-field-delete.md)
- [{#T}](./task-item-user-field-get-types.md)
- [{#T}](./task-item-user-field-get-fields.md)