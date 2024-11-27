# Delegate Task tasks.task.delegate

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not specified
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

The method `tasks.task.delegate` is used for delegating a task

#| 
|| **Parameter** / **Type** | **Description** || 
|| **taskId** 
[`unknown`](../data-types.md) | Task identifier. || 
|| **userId** 
[`unknown`](../data-types.md) | Identifier of the user to whom the task should be delegated. || 
|#

## Example

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'tasks.task.delegate',
        {taskId:1, userId: 2},
        function(res){console.log(res.answer.result);}
    );
    ```

{% endlist %}

{% include [Footnote on examples](../../_includes/examples.md) %}