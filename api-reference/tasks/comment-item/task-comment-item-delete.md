# Delete Comment task.commentitem.delete

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- examples are missing (there should be three examples - curl, js, php)
- response in case of error is missing
- response in case of success is missing
- add a description that access permission can be checked using a special method

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `task.commentitem.delete` deletes a comment. Mandatory authorization via oauth and obtaining an auth code is required.

## Parameters

#|
|| **Parameter** / **Type** | **Description** ||
|| **TASKID^*^**
[`unknown`](../../data-types.md) | Identifier of the task. ||
|| **ITEMID^*^**
[`unknown`](../../data-types.md) | Identifier of the comment. ||
|#

{% include [Footnote about parameters](../../../_includes/required.md) %}

{% note info %}

Maintaining the order of parameters in the request is mandatory. If violated, the request will be executed with errors.

{% endnote %}

## Example

```js
BX24.callMethod(
    'task.commentitem.delete',
    [13, 1205],
    function(result){
        console.info(result.data());
        console.log(result);
    }
);
```
{% include [Footnote about examples](../../../_includes/examples.md) %}