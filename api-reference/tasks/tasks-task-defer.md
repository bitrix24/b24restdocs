# Translate the task to "deferred" status tasks.task.defer

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- examples are missing (there should be three examples - curl, js, php)
- response in case of error is missing
- response in case of success is missing
 
{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will add it soon

{% endnote %}

> Scope: [`task`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `tasks.task.defer` changes the status of a task to "deferred".

#|
|| **Parameter** / **Type** | **Description** ||
|| **taskId**
[`unknown`](../data-types.md) | Identifier of the task. ||
|#

## Example

```js
BX24.callMethod(
    'tasks.task.defer',
    {taskId:1},
    function(res){console.log(res.answer.result);}
);
```

{% include [Footnote on examples](../../_includes/examples.md) %}