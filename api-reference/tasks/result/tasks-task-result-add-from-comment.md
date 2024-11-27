# Add Comment to Result tasks.task.result.addFromComment

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- required parameter is not specified
- examples are missing (there should be three examples - curl, js, php)
- response on success is missing
- response on error is missing

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `tasks.task.result.addFromComment` creates a task result from a comment.

#|
|| **Parameter** / **Type**| **Description** ||
|| **commentId**
[`int`](../../data-types.md) | Comment identifier. ||
|#

## Example

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'tasks.task.result.addFromComment',
        {
            "commentId" : 2549
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.info(result.data());
            }
        }
    );
    ```

{% endlist %}

{% include [Footnote on examples](../../../_includes/examples.md) %}