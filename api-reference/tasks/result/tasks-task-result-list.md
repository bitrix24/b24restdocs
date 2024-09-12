# Get the list of task results tasks.task.result.list

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- the requirement for parameters is not specified
- examples are missing (there should be three examples - curl, js, php)
- response in case of success is missing
- response in case of error is missing

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will add it soon

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `tasks.task.result.list` allows you to view the list of results for a task.

#|
|| **Parameter** / **Type**| **Description** ||
|| **taskId**
[`int`](../../data-types.md) | Task identifier. ||
|#

## Example

```js
BX.ajax.runAction("tasks.task.result.list", {
    data: {
        taskId: 100500
    }
}).then(function (response) { console.log(response);});
```

{% include [Footnote on examples](../../../_includes/examples.md) %}