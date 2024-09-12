# Update Fields of bizproc.activity.update

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- missing parameters or fields
- parameter types not specified
- parameter requirements not specified
- examples missing
- success response missing
- error response missing
- links to pages not yet created are not specified.
  
{% endnote %}

{% endif %}

> Scope: [`bizproc`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method allows updating the fields of an already added action for workflows. The method parameters are similar to [bizproc.activity.add](./bizproc-activity-add.md).

## Example

```javascript
function updateActivity1()
{
    var params = {
        'CODE': 'hash',
        'FIELDS': {
            'DOCUMENT_TYPE': '',
            'FILTER': ''
        },
    };

    BX24.callMethod(
        'bizproc.activity.update',
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

{% include [Footnote on examples](../../../_includes/examples.md) %}