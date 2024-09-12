# Mark checklist item as "incomplete" task.checklistitem.renew

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- examples are missing (there should be three examples - curl, js, php)
- success response is missing
- error response is missing
- add description with hints on how to check access permission using a special method

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `task.checklistitem.renew` marks a completed checklist item as newly active.

## Parameters

#|
|| **Parameter** / **Type**| **Description** ||
|| **TASKID^*^**
[`unknown`](../../data-types.md) | Task identifier. ||
|| **ITEMID^*^**
[`unknown`](../../data-types.md) | Checklist item identifier. ||
|#

{% include [Footnote about parameters](../../../_includes/required.md) %}

{% note info %}

The order of parameters in the request must be followed. If violated, the request will be executed with errors.

{% endnote %}

## Example

```js
BX24.callMethod(
    'task.checklistitem.renew',
    [13, 21],
    function(result){
        console.info(result.data());
        console.log(result);
    }
);
```

{% include [Footnote about examples](../../../_includes/examples.md) %}

## See also

- [task.checklistitem.complete](./task-checklist-item-complete.md)