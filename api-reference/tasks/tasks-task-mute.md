# Enable "Mute" Mode tasks.task.mute

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- examples are missing (there should be three examples - curl, js, php)
- no response in case of error
- no response in case of success
 
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

{% include [Parameter Note](../../_includes/required.md) %}

## Return Value

Returns a JSON array of task data (similar to the method [`tasks.task.get`](./tasks-task-get.md)).

## Example

```js
BX24.callMethod('tasks.task.mute', {id: 1223})
```

{% include [Example Note](../../_includes/examples.md) %}