# List of Active Workflows

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

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

{% note info "bizproc.workflow.instances" %}

{% include notitle [Scope bizproc admin](./_includes/scope-bizproc-admin.md) %}

{% endnote %}

{% note warning "bizproc.workflow.instance.list" %}

There is an old method `bizproc.workflow.instance.list`, which is an alias for the current method `bizproc.workflow.instances`. The method `bizproc.workflow.instance.list` accepts the same parameters and returns the same results. Support for `bizproc.workflow.instance.list` is not guaranteed in the future, so it is recommended to use `bizproc.workflow.instances`.

{% endnote %}

The method returns a list of active workflows.

## Examples

```javascript
BX24.callMethod(
	'bizproc.workflow.instances',
	{
		select: ['ID', 'MODIFIED', 'OWNED_UNTIL', 'MODULE_ID', 'ENTITY', 'DOCUMENT_ID', 'STARTED', 'STARTED_BY', 'TEMPLATE_ID'],
		order: {STARTED: 'DESC'},
		filter: {'>STARTED_BY': 0}
	},
	function(result)
	{
		if(result.error())
			alert("Error: " + result.error());
		else
			console.log(result.data());
	}
);
```
 
{% include [Footnote on examples](../../_includes/examples.md) %}