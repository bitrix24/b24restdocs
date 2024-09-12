# Check Available Actions for Task task.commentitem.isactionallowed

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- examples are missing (there should be three examples - curl, js, php)
- no response in case of error
- no response in case of success

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will add it soon

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `task.commentitem.isactionallowed` checks if the action is allowed.

## Parameters

#|
||  **Parameter** / **Type**| **Description** ||
|| **TASKID^*^**
[`unknown`](../../data-types.md) | Task identifier. ||
|| **ITEMID^*^**
[`unknown`](../../data-types.md) | Comment identifier. ||
|| **ACTIONID^*^**
[`unknown`](../../data-types.md) | Identifier of the action being checked:
- `1` — ACTION_COMMENT_ADD; 
- `2` — ACTION_COMMENT_MODIFY; 
- `3` — ACTION_COMMENT_REMOVE. ||
|#

{% include [Footnote on parameters](../../../_includes/required.md) %}

{% note info %}

The order of parameters in the request is mandatory. If it is violated, the request will be executed with errors.

{% endnote %}

## Example

```js
BX24.callMethod(
    'task.commentitem.isactionallowed',
    [13, 1205, 3],
    function(result){
        console.info(result.data());
        console.log(result);
    }
);
```
{% include [Footnote on examples](../../../_includes/examples.md) %}