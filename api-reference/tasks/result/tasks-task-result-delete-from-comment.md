# Delete Comment from Result tasks.task.result.deleteFromComment

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- required parameters are not specified
- examples are missing (there should be three examples - curl, js, php)
- no response in case of success
- no response in case of error

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `tasks.task.result.deleteFromComment` removes the task result based on the comment from which it was created.

#|
|| **Parameter** / **Type**| **Description** ||
|| **commentId**
[`int`](../../data-types.md) | Identifier of the comment. ||
|#

## Example

```js
BX.ajax.runAction("tasks.task.result.deleteFromComment", {
    data: {
        commentId: 100500
    }
}).then(function (response) { console.log(response);});
```

{% include [Footnote on examples](../../../_includes/examples.md) %}