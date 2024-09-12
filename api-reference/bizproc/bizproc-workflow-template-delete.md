# Deleting a Workflow Template

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- edits needed for writing standards
- missing parameters or fields
- parameter types not specified
- examples missing
- success response missing
- error response missing

{% endnote %}

{% endif %}

{% note info "bizproc.workflow.template.delete" %}

{% include notitle [Scope of bizproc admin](./_includes/scope-bizproc-admin.md) %}

{% endnote %}

Deleting a Workflow Template. This method only removes templates that were created using the [`bizproc.workflow.template.add`](./bizproc-workflow-template-add.md) method, as these templates are tied to the application and only they can be deleted.

#|
|| **Parameter** | **Description** | **Available since** ||
|| **ID**^*^ | Identifier of the template to be deleted. | |
|#	

\* - Required parameters

## Example

```javascript
function deleteTemplate(id)
{
	BX24.callMethod(
		'bizproc.workflow.template.delete',
		{ID: id},
		function(result)
		{
			if(result.error())
				alert("Error: " + result.error());
			console.log(result);
		}
	);
}
```

{% include [Example notes](../../_includes/examples.md) %}