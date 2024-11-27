# Enable "Mute" Mode tasks.task.mute

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- examples are missing (there should be three examples - curl, js, php)
- response in case of error is missing
- response in case of success is missing
 
{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly

{% endnote %}

> Scope: [`task`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `tasks.task.mute` enables "Mute" mode.

#|
|| **Parameter** / **Type** | **Description** ||
|| **id^*^**
[`unknown`](../data-types.md) | Task identifier. ||
|#

{% include [Parameter Notes](../../_includes/required.md) %}

## Return Value

Returns a json array of task data (similar to the method [`tasks.task.get`](./tasks-task-get.md)).

## Example

{% list tabs %}

- JS

    ```js
    BX24.callMethod('tasks.task.mute', {id: 1223})
    ```

{% endlist %}

{% include [Example Notes](../../_includes/examples.md) %}