# Delete Registered Automation Rule bizproc.provider.delete

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- adjustments needed for writing standards
- parameter types not specified
- parameter requirements not indicated
- success response missing
- error response missing

{% endnote %}

{% endif %}

> Scope: [`bizproc`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method deletes a registered Automation rule.

#|
|| Parameters  | Description ||
|| **CODE** | Identifier of the Automation rule. ||
|#

## Examples

```javascript
var params = {
    'CODE': 'robot'
};

BX24.callMethod(
    'bizproc.robot.delete',
    params,
    function(result) {
        if(result.error())
            alert('Error: ' + result.error());
        else
            alert("Success: " + result.data());
    }
);
```

{% include [Example Notes](../../../_includes/examples.md) %}