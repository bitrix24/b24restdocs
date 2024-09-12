# Get Comment by ID task.commentitem.get

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- examples are missing (there should be three examples - curl, js, php)
- response in case of error is missing
- response in case of success is missing

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `task.commentitem.get` returns a comment for a task by its ID.

## Parameters

#|
||  **Parameter** / **Type**| **Description** ||
|| **TASKID^*^**
[`unknown`](../../data-types.md) | Task identifier. ||
|| **ITEMID^*^**
[`unknown`](../../data-types.md) | Comment identifier. ||
|#

{% include [Note on parameters](../../../_includes/required.md) %}

{% note info %}

It is mandatory to follow the order of parameters in the request. If this order is violated, the request will be executed with errors.

{% endnote %}

## Example

```js
BX24.callMethod(
    'task.commentitem.get',
    [13, 1205],
    function(result){
        console.info(result.data());
        console.log(result);
    }
);
```
{% include [Note on examples](../../../_includes/examples.md) %}