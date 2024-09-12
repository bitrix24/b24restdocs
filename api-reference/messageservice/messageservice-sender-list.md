# Get a list of SMS providers or message providers messageservice.sender.list

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameters, types, and required parameters are not specified
- no response in case of success
- no response in case of error
- No examples in other languages

{% endnote %}

{% endif %}

> Scope: [`messageservice`](../scopes/permissions.md)
>
> Who can execute the method: administrator

The method returns a list of message providers registered by the current application (or the same incoming webhook).

## Example

```js
BX24.callMethod(
    'messageservice.sender.list',
    {},
    function(result)
    {
        if(result.error())
            alert("Error: " + result.error());
        else
            alert("Success: " + result.data().join(', '));
    }
);
```
{% include [Note on examples](../../_includes/examples.md) %}