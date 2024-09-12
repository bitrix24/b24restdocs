# Update Task tasks.task.update

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- An additional example with an explanation for linking the task to CRM is needed
- Parameter types are not specified
- Parameter requirements are not indicated
- Examples are missing (there should be three examples - curl, js, php)
- Response in case of an error is missing
- Response in case of success is missing
 
{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will add it soon

{% endnote %}

> Scope: [`task`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `tasks.task.update` updates a task.

#|
|| **Parameter** / **Type** | **Description** ||
|| **taskId**
[`unknown`](../data-types.md) | Task identifier. ||
|| **fields**
[`unknown`](../data-types.md) | Fields corresponding to the available list of fields [tasks.task.getfields](./tasks-task-get-fields.md). ||
|#

## Example

```js
BX24.callMethod(
    'tasks.task.update',
    {taskId:1, fields:{TITLE:'task for test', RESPONSIBLE_ID:1}},
    function(res){console.log(res.answer.result);}
);
```

Method parameters for attaching a file to the task from the drive:

```json
{"taskId": "77", "fields": {"UF_TASK_WEBDAV_FILES": ["n111"]}}
```

where "111" is the file id on the drive.

{% note warning %}

You need to add the letter `n` at the beginning.

{% endnote %}

**Starting from version 22.1300.0**, you can pass the parameter `SE_PARAMETER` to the method - a list of objects with additional task parameters.

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

Code values:

1. deadlines are determined by the deadlines of subtasks
2. automatically complete the task when subtasks are completed (and vice versa)
3. mandatory report upon task completion

{% include [Footnote on examples](../../_includes/examples.md) %}