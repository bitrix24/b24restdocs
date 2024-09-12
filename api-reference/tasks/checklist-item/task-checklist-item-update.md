# Update Checklist Item task.checklistitem.update

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- examples are missing (there should be three examples - curl, js, php)
- success response is missing
- error response is missing
- add a description with hints on how to check access permission for modification using a special method

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `task.checklistitem.update` updates the data of a checklist item.

It is recommended to check if this action is allowed before updating the data ([task.checklistitem.isactionallowed](./task-checklist-item-is-action-allowed.md)).

## Parameters

#|
|| **Parameter** / **Type**| **Description** ||
|| **TASKID^*^**
[`unknown`](../../data-types.md) | Task identifier. ||
|| **ITEMID^*^**
[`unknown`](../../data-types.md) | Checklist item identifier. ||
|| **FIELDS^*^**
[`unknown`](../../data-types.md) | Array of checklist item fields (**TITLE**, **SORT_INDEX**, **IS_COMPLETE**). ||
|#

{% include [Footnote about parameters](../../../_includes/required.md) %}

{% note info %}

The order of parameters in the request is mandatory. If violated, the request will be executed with errors.

{% endnote %}

## Example

```js
// Updating the item with ID=25 status to "not completed", and text to "Item not completed"
BX24.callMethod(
    'task.checklistitem.update',
    [13, 25, {'TITLE': 'Item not completed', 'IS_COMPLETE': 'N'}],
    function(result){
        console.info(result.data());
        console.log(result);
    }
);
```

{% include [Footnote about examples](../../../_includes/examples.md) %}