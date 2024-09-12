# Deleting a Running Process

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- adjustments needed for writing standards
- parameter types not specified
- examples are missing
- success response is absent
- error response is absent
- what is the difference from the terminate method?
  
{% endnote %}

{% endif %}

{% note info "bizproc.workflow.kill!" %}

{% include notitle [Scope of bizproc all](./_includes/scope-bizproc-all.md) %}

{% endnote %}

This method deletes a running workflow.

#|
|| **Parameter** | **Description** ||
|| **ID**^*^ | Identifier of the workflow. ||
|#

\* - Required parameters

## Example
 ```javascript
function killWf(id, cb)
{
	var params = {ID: id};
	BX24.callMethod(
		'bizproc.workflow.kill',
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