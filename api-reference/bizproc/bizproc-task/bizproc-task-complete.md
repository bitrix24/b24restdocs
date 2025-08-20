# Complete a business process task bizproc.task.complete

> Scope: [`bizproc`](../../scopes/permissions.md)
>
> Who can execute the method: any user

This method completes a business process task:
- Document approval
- Document acknowledgment
- Request for additional information
- Request for additional information with rejection
  
You can only complete your own task.

{% note tip "User documentation" %}

- [Actions: Tasks](https://helpdesk.bitrix24.com/open/11466058/)

{% endnote %}

## Method parameters

{% include [Footnote about parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **TASK_ID***
[`integer`](../../data-types.md) | Task identifier.

You can obtain the identifier using the [bizproc.task.list](./bizproc-task-list.md) method. ||
|| **STATUS***
[`integer` \| `string`](../../data-types.md) | Target status of the task. Possible values: 

- `1` or `yes` — yes, approved
- `2` or `no` — no, rejected
- `3` or `ok` — ok, acknowledged
- `4` or `cancel` — cancellation

The set of acceptable values changes depending on the type of task:
- Document approval — `1` or `2`
- Document acknowledgment — `3`
- Request for additional information — `3`
- Request for additional information with rejection — `3` or `4`
||
|| **COMMENT**
[`string`](../../data-types.md) | User comment.

The requirement for this parameter depends on the task settings. ||
|| **FIELDS**
[`object`](../../data-types.md) | An object describing fields for completing tasks with a request for additional information in the format `{"field_1": "value_1", ... "field_N": "value_N"}`, where
- `field_N` — symbolic identifier of the task field
- `value_N` — field value

You can obtain field descriptions in the task using the [bizproc.task.list](./bizproc-task-list.md) method in the `"PARAMETERS": "Fields"` object of the response. The structure of the field object description:

```json
"PARAMETERS": {
    ...
    "Fields": [
        {
            "Id": "field_id",
            "Type": "type",
            "Name": "name",
            "Description": "description",
            "Multiple": false,
            "Required": true,
            "Options": null,
            "Settings": null,
            "Default": "default_value"
        }
```

`Id` — symbolic identifier of the task field.

The `Default` contains default values that can be passed for task completion. These values are converted to an external representation:
- for dates — in the rest ATOM ISO-8601 format
- for files — as a link to the file 

Values are passed in this format to the `bizproc.task.complete` method. They are then converted to an internal representation:
- dates from the rest format are converted to internal
- files are saved and attached to the business process

To pass a value in a File type field, specify:
- for File type — base64 or an array with the name and base64
- for File type (Drive) — file identifier from the Drive

More about working with files can be found in the article [{#T}](../../files/how-to-upload-files.md)

||
|#

## Code example

{% include [Footnote about examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASK_ID":1501,"STATUS":1,"COMMENT":"Added","Fields":{"contractor":"C_607","phone_number":"+19991234567"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/bizproc.task.complete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASK_ID":1501,"STATUS":1,"COMMENT":"Added","Fields":{"contractor":"C_607","phone_number":"+19991234567"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/bizproc.task.complete
    ```

- JS

    ```js
    BX24.callMethod(
        'bizproc.task.complete',
        {
            'TASK_ID': 1501,
            'STATUS': 1,
            'COMMENT': 'Added',
            "Fields": {
                'contractor': 'C_607',
                'phone_number': '+19991234567'
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
        'bizproc.task.complete',
        [
            'TASK_ID' => 1501,
            'STATUS' => 1,
            'COMMENT' => 'Added',
            'Fields' => [
                'contractor' => 'C_607',
                'phone_number' => '+19991234567'
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response handling
 
HTTP status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1738746693.9218969,
        "finish": 1738746694.1367991,
        "duration": 0.21490216255187988,
        "processing": 0.19237995147705078,
        "date_start": "2025-02-05T12:11:33+01:00",
        "date_finish": "2025-02-05T12:11:34+01:00",
        "operating_reset_at": 1738747293,
        "operating": 0.19236207008361816
    }
}
```

### Returned data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Returns `true` if the task was completed successfully. ||
|| **time**
[`time`](../../data-types.md#time) | Information about the time taken for the request. ||
|#

## Error handling

HTTP status: **400**

```json
{
    "error": "ERROR_TASK_VALIDATION",
    "error_description": "incorrect STATUS"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible error codes
 
#|
|| **Code** | **Error message** | **Description** ||
|| `ERROR_TASK_VALIDATION` | empty TASK_ID | `ID` of the task is not specified. ||
|| `ERROR_TASK_VALIDATION` | incorrect STATUS | An incorrect task status was specified. ||
|| `ERROR_TASK_NOT_FOUND` | Task not found | No task found with the specified `ID`. ||
|| `ERROR_TASK_COMPLETED` | Task already completed | The task has already been completed. ||
|| `ERROR_TASK_TYPE` | Incorrect task type | Incorrect task type. This task cannot be completed via REST. ||
|| `ERROR_TASK_EXECUTION` | error text from the task | An error occurred during the execution of the task. ||
|#
 
 {% include [system errors](../../../_includes/system-errors.md) %}

 ## Continue learning 
 
 - [{#T}](./index.md)
 - [{#T}](./bizproc-task-list.md)
 - [{#T}](./bizproc-task-delegate.md)