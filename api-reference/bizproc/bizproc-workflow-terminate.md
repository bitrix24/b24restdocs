# Stopping an Active Workflow

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not to be deployed to prod_" %}

- edits needed for writing standards
- parameter types not specified
- parameter requirements not indicated
- examples missing
- success response not provided
- error response not provided
- links to yet-to-be-created pages not documented.

{% endnote %}

{% endif %}

{% note info "bizproc.workflow.terminate" %}

{% include notitle [Scope of bizproc all](./_includes/scope-bizproc-all.md) %}

{% endnote %}

This method stops the specified workflow.

#|
|| **Parameter** | **Description** ||
|| **ID**^*^ | Identifier of the workflow to be stopped. The identifier can be obtained using the method [bizproc.workflow.instances](./bizproc-workflow-instances.md) ||
|| **STATUS** | Set the status text ||
|#

## Examples

```javascript
function terminateWf(id, cb)
{
	var params = {ID: id, STATUS: 'Terminated by rest app.'};
	BX24.callMethod(
		'bizproc.workflow.terminate',
		params,
		function(result)
		{
			if(result.error())
				alert("Error: " + result.error());
			else if (cb)
				cb();
		}
	);
}
```

{% include [Footnote on examples](../../_includes/examples.md) %}