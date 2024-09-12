# Enable Task Monitoring tasks.task.startwatch

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- examples are missing (there should be three examples - curl, js, php)
- no error response is provided
- no success response is provided
 
{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will add it soon

{% endnote %}

> Scope: [`task`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `tasks.task.startwatch` allows you to monitor a task.

#|
|| **Parameter** / **Type** | **Description** ||
|| **taskId**
[`unknown`](../data-types.md) | Task identifier. ||
|#

## Example

```js
BX24.callMethod(
    'tasks.task.startwatch',
    {taskId:1},
    function(res){console.log(res.answer.result);}
);
```

{% include [Footnote on examples](../../_includes/examples.md) %}