# Get checklist item by ID task.checklistitem.get

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

The method `task.checklistitem.get` returns a checklist item by its ID.

## Parameters

#|
|| **Parameter** / **Type**| **Description** ||
|| **TASKID^*^**
[`unknown`](../../data-types.md) | Task identifier. ||
|| **ITEMID^*^**
[`unknown`](../../data-types.md) | Checklist item identifier. ||
|#

{% include [Note on parameters](../../../_includes/required.md) %}

{% note info %}

The order of parameters in the request is mandatory. If violated, the request will be executed with errors.

{% endnote %}

## Example

```js
BX24.callMethod(
    'task.checklistitem.get',
    [13, 20],
    function(result){
        console.info(result.data());
        console.log(result);
    }
);
```

{% include [Note on examples](../../../_includes/examples.md) %}