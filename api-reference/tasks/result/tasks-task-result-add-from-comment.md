# Add Comment to Result tasks.task.result.addFromComment

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- the required parameter is not specified
- examples are missing (there should be three examples - curl, js, php)
- response on success is missing
- response on error is missing

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `tasks.task.result.addFromComment` creates a task result from a comment.

#|
|| **Parameter** / **Type**| **Description** ||
|| **commentId**
[`int`](../../data-types.md) | Identifier of the comment. ||
|#

## Example

```js
BX.ajax.runAction("tasks.task.result.addFromComment", {
    data: {
        commentId: 100500
    }
}).then(function (response) { console.log(response);});
```

{% include [Footnote on examples](../../../_includes/examples.md) %}