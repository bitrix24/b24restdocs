# Update the business process template bizproc.workflow.template.update

> Scope: [`bizproc`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method updates a business process template.

It allows you to update templates that were created using the method [bizproc.workflow.template.add](./bizproc-workflow-template-add.md). These templates are tied to the application and can only be updated in the context of the same [application](../../app-installation/index.md) that created them.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description**||
|| **ID***
[`string`](../../data-types.md) | Identifier of the template to be updated ||
|| **FIELDS***
[`object`](../../data-types.md) | Object with [fields](#parametr-fields) of the business process template.

You can update the fields: `NAME`, `DESCRIPTION`, `TEMPLATE_DATA`, `AUTO_EXECUTE`. Attempting to update [other fields](./bizproc-workflow-template-list.md#fields) of the template will not result in errors, but those fields will not be updated ||
|#

### FIELDS Parameter {#parametr-fields}

#|
|| **Name**
`type` | **Description**||
|| **NAME**
[`string`](../../data-types.md) | Name of the template ||
|| **DESCRIPTION**
[`string`](../../data-types.md) | Description of the template ||
|| **TEMPLATE_DATA**
[`file`](../../data-types.md) | Content of the file with the business process template in `.bpt` format.

More about file transfer methods can be found in the article [{#T}](../../bx24-js-sdk/how-to-call-rest-methods/files.md) ||
|| **AUTO_EXECUTE**
[`integer`](../../data-types.md) | Auto-execution settings of the template. Can have the value:

- `0` — no auto-execution
- `1` — execute on creation
- `2` — execute on modification
- `3` — execute on creation and modification

Default is `0` ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ID":525,"FIELDS":{"NAME":"Display Time","DESCRIPTION":"Template shows a message with local and server time","AUTO_EXECUTE":0},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/bizproc.workflow.template.update
    ```

- JS

    ```js
    BX24.callMethod(
        'bizproc.workflow.template.update',
        {
            ID: 525,
            FIELDS: {
                NAME: 'Display Time',
                DESCRIPTION: 'Template shows a message with local and server time',
                AUTO_EXECUTE: 0,
            }
        },
        function(result)
        {
            if(result.error())
                alert("Error: " + result.error());
            else
                console.log(result.data());
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'bizproc.workflow.template.update',
        [
            'ID' => 525,
            'FIELDS' => [
                'NAME' => 'Display Time',
                'DESCRIPTION' => 'Template shows a message with local and server time',
                'AUTO_EXECUTE' => 0
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": 525,
    "time": {
        "start": 1737534421.172673,
        "finish": 1737534421.209758,
        "duration": 0.037085056304931641,
        "processing": 0.010869026184082031,
        "date_start": "2025-01-22T11:27:01+01:00",
        "date_finish": "2025-01-22T11:27:01+01:00",
        "operating_reset_at": 1737535021,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Identifier of the updated business process template ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ERROR_TEMPLATE_NOT_OWNER",
    "error_description": "You can update ONLY templates created by current application",
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Error Message** | **Description** ||
|| `ACCESS_DENIED` | Application context required | Access token not from the application ||
|| `ACCESS_DENIED` | Access denied! | Method was not executed by an administrator ||
|| `ERROR_TEMPLATE_VALIDATION_FAILURE` | No fields to update. | No fields specified for update ||
|| `ERROR_TEMPLATE_NOT_FOUND` | Workflow template not found. | Template with the specified `ID` not found ||
|| `ERROR_TEMPLATE_NOT_OWNER` | You can update ONLY templates created by current application | This template cannot be edited through this application ||
|| `ERROR_TEMPLATE_VALIDATION_FAILURE` | Empty template name! | An empty template name was specified ||
|| `ERROR_TEMPLATE_VALIDATION_FAILURE` | Incorrect field AUTO_EXECUTE! | An incorrect auto-execution code was specified ||
|| `ERROR_TEMPLATE_VALIDATION_FAILURE` | Incorrect field TEMPLATE_DATA! | Incorrect template data was specified ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./bizproc-workflow-template-add.md)
- [{#T}](./bizproc-workflow-template-list.md)
- [{#T}](./bizproc-workflow-template-delete.md)