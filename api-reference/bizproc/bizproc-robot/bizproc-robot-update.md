# Update Robot Fields bizproc.robot.update

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- missing parameters or fields
- parameter types not specified
- required parameters not indicated
- success response missing
- error response missing

{% endnote %}

{% endif %}

> Scope: [`bizproc`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method updates the fields of the Automation rule. The same parameters used in [bizproc.robot.add](./bizproc-robot-add.md) are passed in the **FIELDS** array.

## Examples

```js
function updateRobot1()
{
    var params = {
        'CODE': 'hash',
        'FIELDS': {
            'DOCUMENT_TYPE': '',
            'FILTER': ''
        },
    };
    BX24.callMethod(
        'bizproc.robot.update',
        params,
        function(result)
        {
            if(result.error())
                alert("Error: " + result.error());
            else
                alert("Successfully: " + result.data());
        }
    );
}
```

{% include [Note on examples](../../../_includes/examples.md) %}