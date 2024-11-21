# Complete Business Process Tasks bizproc.task.complete

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- A comprehensive table of parameters needs to be compiled, including PARAMETERS.
- Details of the values for PARAMETERS and STATUS should be moved to separate tables.
- It's unclear how the developer should know which specific Fields they can or must fill out.
- Examples are lacking.
- There are no standard blocks.

{% endnote %}

{% endif %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- Edits are needed to conform to writing standards.
- Parameter types are not specified.
- Examples are missing.
- There is no response in case of success.
- There is no response in case of error.
- Links to pages that have not yet been created are not provided.

{% endnote %}

{% endif %}

> Scope: [`bizproc`](../../scopes/permissions.md)
>
> Who can execute the method: any user

This method executes the specified business process task. Currently, tasks of the following types can be completed:

- [Document Approval](https://dev.1c-bitrix.com/learning/course/index.php?COURSE_ID=57&LESSON_ID=3771)
- [Document Acknowledgment](https://dev.1c-bitrix.com/learning/course/index.php?COURSE_ID=57&LESSON_ID=3783)
- [Request for Additional Information](https://dev.1c-bitrix.com/learning/course/index.php?COURSE_ID=57&LESSON_ID=3782)

You can only complete your own task (related to the user whose access token is used to execute the method) if it has not already been completed.

#|
|| **Parameter** | **Description** | **Note** | **Available since** ||
|| **TASK_ID^*^** | Task identifier, required | |  ||
|| **STATUS^*^** | Target status of the task, required. List of acceptable values: 

- `1` or yes - response "Yes" (approved)
- `2` or no - response "No" (rejected)
- `3` or ok - response "Ok" (acknowledged)
- `4` or cancel - response "Cancel" | Statuses: **1** and **2** for Document Approval action;  **3** and **4** for Request for Additional Information action; **3** for Request for Additional Information with rejection. |  ||
|| **COMMENT** | User comment, required depending on task parameters | |  ||
|#

{% include [Parameter Notes](../../../_includes/required.md) %}

## Example

```js
function completeTask(id, status, comment, cb)
{
    var params = {
        TASK_ID: id,
        STATUS: status,
        COMMENT: comment
    };
    BX24.callMethod(
        'bizproc.task.complete',
        params,
        function(result)
        {
            if(result.error())
                alert("Error: " + result.error());
            else if (cb)
                cb();
        }
    );
}
```

## Completing the Request for Additional Information Task via REST

Starting from version **20.0.800** of the Business Processes module, it became possible to complete **Request for Additional Information** tasks via the REST method **bizproc.task.complete**.

To understand which fields need to be filled out, a new property `Fields` has been added to PARAMETERS in the method [bizproc.task.list](.) - an array describing the fields.

```json
"PARAMETERS": {
    "CommentLabel": "Comment",
    "CommentRequired": "N",
    "ShowComment": "Y",
    "StatusOkLabel": "Save",
    "Fields": [
        {
            "Type": "datetime",
            "Name": "date",
            "Description": "",
            "Multiple": false,
            "Required": true,
            "Options": null,
            "Settings": null,
            "Default": "2020-07-08T15:16:12+02:00",
            "Id": "date"
        }
    ]
}
```

Default values are stored in the **Default** section. Values are converted to "external" representation (for dates - to REST ATOM format (ISO-8601), and for files - to a link to the file).

Next, the values of these fields need to be passed to the **bizproc.task.complete** method in the **Fields** parameter. This time, the values are converted to "internal representation" (i.e., dates from REST format are converted to internal format, and files from REST are saved and attached to the business process).

{% include [Example Notes](../../../_includes/examples.md) %}