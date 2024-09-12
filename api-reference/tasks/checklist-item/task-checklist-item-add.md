# Add Checklist Item task.checklistitem.add

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not specified
- one example is missing (there should be three examples - curl, js, php)
- success response is missing
- error response is missing
- add description with hints on how to check access permission using a special method

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `task.checklistitem.add` adds a new checklist item to a task. It returns the identifier of the added item. The method does not grant access permissions to the task for the user specified in the `FIELDS` array, as is done through the B24 interface. If the user did not have view permissions, their appearance in the checklist in any role will not make the task accessible.

## Parameters

#|
|| **Parameter** / **Type**| **Description** ||
|| **TASKID**
[`unknown`](../../data-types.md) | Task identifier. Required parameter. ||
|| **FIELDS**
[`unknown`](../../data-types.md) | Array of checklist item fields (`TITLE`, `SORT_INDEX`, `IS_COMPLETE`). Required parameter. ||
|#

{% note info %}

Maintaining the order of parameters in the request is mandatory. If violated, the request will be executed with errors.

{% endnote %}

## Examples

{% list tabs %}

- JS

    ```js
    // Adding a new "completed" checklist item with the text "Item completed" to the task with ID=13
    BX24.callMethod(
        'task.checklistitem.add',
        [13, {'TITLE': 'Item completed', 'IS_COMPLETE': 'Y'}],
        function(result){
            console.info(result.data());
            console.log(result);
        }
    );
    ```

- PHP

    ```php
    // To add a user to the checklist item:
    $fields = [
        'TITLE' => "Checklist item title User Name",//$user['NAME']." ".$user['LAST_NAME'], order depending on account settings.
        'IS_COMPLETE' => 'N',
        'IS_IMPORTANT' => 'N',
        'MEMBERS' => [13 => ['TYPE' => 'A']], //TYPE - role_in_checklist: A - Participant, U - observer
        'PARENT_ID' => '$result[0]' // for batch call, otherwise just the ID of the parent item
    ];
    ```

{% endlist %}

{% include [Examples note](../../../_includes/examples.md) %}