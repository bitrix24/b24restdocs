# Move Checklist Item task.checklistitem.moveafteritem

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

The method `task.checklistitem.moveafteritem` moves a checklist item in the list after the specified one.

## Parameters

#|
|| **Parameter** / **Type**| **Description** ||
|| **TASKID^*^**
[`unknown`](../../data-types.md) | Task identifier. ||
|| **ITEMID^*^**
[`unknown`](../../data-types.md) | Checklist item identifier. ||
|| **AFTERITEMID^*^**
[`unknown`](../../data-types.md) | Identifier of the checklist item after which the specified item will be placed. ||
|#

{% include [Footnote about parameters](../../../_includes/required.md) %}

{% note info %}

Maintaining the order of parameters in the request is mandatory. If violated, the request will be executed with errors.

{% endnote %}

## Example

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'task.checklistitem.moveafteritem',
        [13, 21, 9],
        function(result){
            console.info(result.data());
            console.log(result);
        }
    );
    ```

{% endlist %}

{% include [Footnote about examples](../../../_includes/examples.md) %}