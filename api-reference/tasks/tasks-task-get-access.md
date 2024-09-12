# Check Access to Task tasks.task.getaccess

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

Some data may be missing here — we will fill it in shortly

{% endnote %}

> Scope: [`task`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `tasks.task.getaccess` is used to check access to a task.

#|
|| **Parameter** / **Type** | **Description** ||
|| **taskId**
[`unknown`](../data-types.md) | Task identifier. ||
|| **users**
[`unknown`](../data-types.md) | Array of user IDs for whom access needs to be checked. By default, the current user is used. ||

|#

## Example

```js
BX24.callMethod(
    'tasks.task.getaccess',
    {taskId:1, users:[1]},
    function(res){console.log(res.answer.result);}
);
```

{% include [Footnote on examples](../../_includes/examples.md) %}