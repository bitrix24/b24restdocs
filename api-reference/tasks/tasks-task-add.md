# Add Task tasks.task.add

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- An additional example with an explanation of linking the task to CRM is needed
- Parameter types are not specified
- Required parameters are not indicated
- Examples are missing (there should be three examples - curl, js, php)
- Response in case of an error is missing
- Response in case of success is missing
 
{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly

{% endnote %}

> Scope: [`task`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `tasks.task.add` creates a task. 

#|
|| **Parameter** / **Type** | **Description** ||
|| **fields**
[`unknown`](../data-types.md) | Fields corresponding to the available list of fields [tasks.task.getfields](./tasks-task-get-fields.md). ||
|#

## Examples

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'tasks.task.add',
        {fields:{TITLE:'task for test', RESPONSIBLE_ID:1}},
        function(res){console.log(res.answer.result);}
    );
    ```

{% endlist %}

To attach a file to the task, the file identifier must be preceded by the character `n`

{% list tabs %}

- JS

    ```js
    {
        "taskId":"76",
        "fields": {
            "UF_TASK_WEBDAV_FILES": [
                "n96"
            ]
        }
    }
    ```

{% endlist %}

**Starting from version 22.1300.0**, you can pass the parameter `SE_PARAMETER` to the method — a list of objects with additional task parameters.

{% list tabs %}

- JS

    ```js
    BX.ajax.runAction("tasks.task.add", {
        data: {
            fields: {
                "TITLE": 'REST',
                "RESPONSIBLE_ID": 1,
                "SE_PARAMETER": [
                    {
                        'VALUE': 'Y',
                        'CODE': 3
                    },
                    {
                        'VALUE': 'Y',
                        'CODE': 2
                    },
                ]
            }
        }
    }).then(function (response) { console.log(response);});
    ```

{% endlist %}

Code values:

1. deadlines are determined by the deadlines of subtasks
2. automatically complete the task when subtasks are completed (and vice versa)
3. mandatory report upon task completion

{% include [Footnote on examples](../../_includes/examples.md) %}

## Continue Learning

- [{#T}](../../tutorials/tasks/how-to-create-task-with-file.md)