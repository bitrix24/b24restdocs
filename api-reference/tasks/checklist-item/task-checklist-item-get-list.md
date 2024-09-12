# Get the list of checklist items task.checklistitem.getlist

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- examples are missing (there should be three examples - curl, js, php)
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `task.checklistitem.getlist` returns a list of checklist items in a task.

## Parameters

#|
|| **Parameter** / **Type**| **Description** ||
|| **TASKID^*^**
[`unknown`](../../data-types.md) | Task identifier. ||
|| **ORDER**
[`unknown`](../../data-types.md) | Array for sorting the result. The sorting field can take the following values:
- `ID` — checklist item identifier;
- `CREATED_BY` — identifier of the user who created the item;
- `TOGGLED_BY` — identifier of the user who changed the state of the checklist item;
- `TOGGLED_DATE` — time when the state of the checklist item was changed;
- `TITLE` — title of the checklist item;
- `SORT_INDEX` — sorting index of the item;
- `IS_COMPLETE` — item marked as completed.

The sorting direction can take the following values:
- `asc` — ascending;
- `desc` — descending.

Optional. By default, it is filtered in descending order by the checklist item identifier. ||
|#

{% include [Parameter note](../../../_includes/required.md) %}

{% note info %}

Maintaining the order of parameters in the request is mandatory. If violated, the request will be executed with errors.

{% endnote %}

## Example

```js
BX24.callMethod(
    'task.checklistitem.getlist',
    [13, {'TOGGLED_DATE': 'desc'}],
    function(result){
        console.info(result.data());
        console.log(result);
    }
);
```

{% include [Example note](../../../_includes/examples.md) %}