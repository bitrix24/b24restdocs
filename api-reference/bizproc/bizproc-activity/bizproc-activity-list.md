# Get a List of Installed Actions bizproc.activity.list

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- missing parameters or fields
- parameter types not specified
- required parameters not indicated
- examples missing
- success response missing
- error response missing

{% endnote %}

{% endif %}

> Scope: [`bizproc`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method returns a list of actions installed by the application.

## Examples

```javascript
BX24.callMethod(
    'bizproc.activity.list',
    {},
    function(result) {
        if(result.error())
            alert("Error: " + result.error());
        else
            alert("Success: " + result.data().join(', '));
    }
);
```

{% include [Note on Examples](../../../_includes/examples.md) %}