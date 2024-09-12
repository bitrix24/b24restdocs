# Get a list of methods and their descriptions task.checklistitem.getmanifest

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- missing examples (there should be three examples - curl, js, php)
- missing response in case of success
- missing response in case of error

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `task.checklistitem.getmanifest` returns a list of methods of the form `task.checklistitem.*` and their descriptions.

The return value of this method is not intended for automated processing, as its format may change without notice.

The method can be useful as reference information, as it always contains up-to-date information.

{% note info %}

It is mandatory to follow the order of parameters in the request. If this order is violated, the request will be executed with errors.

{% endnote %}

## Example

```js
BX24.callMethod(
    'task.checklistitem.getmanifest',
    [],
    function(result)
    {
        console.info(result.data());
        console.log(result);
    }
);
```

{% include [Footnote on examples](../../../_includes/examples.md) %}