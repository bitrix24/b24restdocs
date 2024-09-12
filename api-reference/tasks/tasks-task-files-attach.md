# Attach Files to Task tasks.task.files.attach

{% if build == 'dev' %}

{% note alert "TO-DO _not uploaded to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- examples are missing (there should be three examples - curl, js, php)
- no response in case of error
- no response in case of success
 
{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon

{% endnote %}

> Scope: [`task`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `tasks.task.files.attach` is used to attach a file uploaded to the disk to a task.

#|
|| **Parameter** / **Type** | **Description** ||
|| **taskId**
[`unknown`](../data-types.md) | Identifier of the task. ||
|| **fileId**
[`unknown`](../data-types.md) | Identifier of the uploaded file. ||
|| **params**
[`unknown`](../data-types.md) | Array of additional parameters, empty by default. Currently not used. ||
|#

## Example

```js
BX24.callMethod(
    'tasks.task.files.attach',
    {
        taskId: 1,
        fileId: 1065,
    },
    function(res) {
        console.log(res.answer.result);
    }
);
```

{% include [Footnote on examples](../../_includes/examples.md) %}