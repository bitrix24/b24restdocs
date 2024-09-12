# Renew Task After Completion tasks.task.renew

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

Some data may be missing here — we will complete it soon

{% endnote %}

> Scope: [`task`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `tasks.task.renew` renews a task after its completion.

## Parameters

#|
|| **Parameter** / **Type** | **Description** ||
|| **taskId**
[`unknown`](../data-types.md) | Task identifier. ||
|#

## Example

```js
BX24.callMethod(
    'tasks.task.renew',
    {taskId:1},
    function(res){console.log(res.answer.result);}
);
```

{% include [Footnote on examples](../../_includes/examples.md) %}