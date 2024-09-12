# Get a list of registered application Automation rules bizproc.robot.list

{% note warning "We are still updating this page" %}

Some data may be missing here — we will add it soon

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

The method returns a list of Automation rules registered by the application.

## Examples

```javascript
BX24.callMethod(
	'bizproc.robot.list',
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

{% include [Footnote about examples](../../../_includes/examples.md) %}