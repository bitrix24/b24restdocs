# Task History `tasks.task.history.list`

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not specified
- no response in case of error

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon

{% endnote %}

> Scope: [`task`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `tasks.task.history.list` retrieves the history of a task.

It returns an array of data (see examples below).

You can filter and sort by all fields (see [tasks.task.list](./tasks-task-list.md)). By default, it returns the entire history without pagination.

#|
|| **Parameter** / **Type** | **Description** ||
|| **taskId**
[`unknown`](../data-types.md) | Task identifier. ||
|| **start**
[`unknown`](../data-types.md) | How many initial records to skip in the result. Due to technical limitations, this parameter's value must always be a multiple of 50. For example, with a value of 50, the result will display the 51st record and subsequent ones, while the first 50 records will be skipped.

With a value of -1, the count will be disabled.

Works for HTTPS requests.||
|#

## Example 1

Output the history of a specific task using the `NEW` filter (i.e., when the task was created):
```js
BX24.callMethod('tasks.task.history.list', {taskId: 119, filter:{FIELD:'NEW'}}, (res)=>{console.log(res.answer.result);});
```

## Successful Response

> 200 OK

```json
{
    "result": {
        "list": [
            {
                "id": "1230",
                "createdDate": "03.01.2019 15:29:28",
                "field": "NEW",
                "value": {
                    "from": null,
                    "to": null
                },
                "user": {
                    "id": "1",
                    "name": "Max",
                    "lastName": "Grechushnikov",
                    "secondName": "",
                    "login": "admin"
                }
            }
        ]
    },
    "time": {
        "start": 1552382093.81029,
        "finish": 1552382093.927268,
        "duration": 0.11697793006896973,
        "processing": 0.018744230270385742,
        "date_start": "2019-03-12T11:14:53+02:00",
        "date_finish": "2019-03-12T11:14:53+02:00"
    }
}
```

## Example 2

Output the history of a specific task without using filters:

```js
BX24.callMethod(
    'task.planner.getlist',
    [],
    function(result)
    {
        console.info(result.data());
        console.log(result);
    }
);
```

{% include [Footnote on examples](../../_includes/examples.md) %}