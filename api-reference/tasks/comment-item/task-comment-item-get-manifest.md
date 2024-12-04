# Get the list of methods and their description task.commentitem.getmanifest

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- examples are missing (there should be three examples - curl, js, php)
- no response in case of error
- no response in case of success
- add a list of fields

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `task.commentitem.getmanifest` returns a list of methods of the form `task.commentitem.*` and their descriptions.

The return value of this method is not intended for automated processing, as its format may change without notice.

The method can be useful as reference information, as it always contains up-to-date information.

{% note info %}

Maintaining the order of parameters in the request is mandatory. If violated, the request will be executed with errors.

{% endnote %}

## Example

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'task.commentitem.getmanifest',
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