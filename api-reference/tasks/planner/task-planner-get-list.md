# Get the list of tasks from the Planner for the day task.planner.getlist

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- Are there no parameters for the method?
- Missing examples (there should be three examples - curl, js, php)
- Missing response in case of an error
- Missing response in case of success
 
{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `task.planner.getlist` returns an array containing the identifiers of tasks in the [Planner for the day](https://helpdesk.bitrix24.com/open/18068292/).

{% note info %}

It is mandatory to follow the order of parameters in the request. If this order is violated, the request will be executed with errors.

{% endnote %}

## Example

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'task.planner.getlist',
        [],
        function(result)
        {
            console.info(result.data());
            console.log(result);
        }
    );
    ```

{% endlist %}

{% include [Footnote on examples](../../../_includes/examples.md) %}