# Check if the action is allowed with task.commentitem.isactionallowed

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- examples are missing (there should be three examples - curl, js, php)
- no response in case of error
- no response in case of success

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon

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

{% include [Note on parameters](../../../_includes/required.md) %}

{% note info %}

The order of parameters in the request must be followed. If violated, the request will be executed with errors.

{% endnote %}

## Example

{% list tabs %}

- JS

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

{% endlist %}

{% include [Note on examples](../../../_includes/examples.md) %}