# Remove Task from Favorites tasks.task.favorite.remove

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not specified
- examples are missing (there should be three examples - curl, js, php)
- no error response provided
 
{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon

{% endnote %}

> Scope: [`task`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `tasks.task.favorite.remove` removes a task from Favorites.

Upon successful execution, it returns the parameter `true` (otherwise `false`).


#|
|| **Parameter** / **Type** | **Description** ||
|| **taskId**
[`unknown`](../data-types.md) | Task identifier. ||
|#

## Example

```js
BX24.callMethod('tasks.task.favorite.remove', {taskId: 119}, (res)=>{console.log(res.answer.result);});
```

{% include [Examples Note](../../_includes/examples.md) %}

## Success Response

> 200 OK

```json
{
    "result": true,
    "time": {
        "start": 1552382402.930095,
        "finish": 1552382403.055257,
        "duration": 0.12516212463378906,
        "processing": 0.09590816497802734,
        "date_start": "2019-03-12T11:20:02+02:00",
        "date_finish": "2019-03-12T11:20:03+02:00"
    }
}
```