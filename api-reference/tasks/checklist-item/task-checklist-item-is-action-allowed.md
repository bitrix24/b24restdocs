# Check Action Permission for task.checklistitem.isactionallowed

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
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

The method `task.checklistitem.isactionallowed` checks whether the action is permitted.

## Parameters

#|
|| **Parameter** / **Type**| **Description** ||
|| **TASKID^*^**
[`unknown`](../../data-types.md) | Task identifier. ||
|| **ITEMID^*^**
[`unknown`](../../data-types.md) | Checklist item identifier. ||
|| **ACTIONID^*^**
[`unknown`](../../data-types.md) | Identifier of the action being checked:
- **1** - ACTION_TIME_ADD;
- **2** - ACTION_MODIFY;
- **3** - ACTION_REMOVE;
- **4** - ACTION_TOGGLE. ||
|#

{% include [Footnote about parameters](../../../_includes/required.md) %}

## Example

{% list tabs %}

- JS

    ```js
    // For the item with ID=21, check if the modification action is allowed
    BX24.callMethod(
        'task.checklistitem.isactionallowed',
        [13, 21, 2],
        function(result){
            console.info(result.data());
            console.log(result);
        }
    );
    ```

{% endlist %}

{% include [Footnote about examples](../../../_includes/examples.md) %}