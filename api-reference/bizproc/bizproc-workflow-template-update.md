# Updating a Workflow Template

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- revisions needed for writing standards
- parameter types not specified
- parameter requirements not indicated
- examples missing
- success response missing
- error response missing

{% endnote %}

{% endif %}

{% note info "bizproc.workflow.template.update" %}

{% include notitle [Scope bizproc admin](./_includes/scope-bizproc-admin.md) %}

{% endnote %}

This method updates only those templates that were created using the method [bizproc.workflow.template.add](./bizproc-workflow-template-add.md), as such templates are tied to the application and only they can be updated.

#|
|| **Parameter** | **Description** | **Version** ||
|| **ID** | Identifier of the template to be modified. | ||
|| **FIELDS** | Array of modifiable parameters. You can update the fields: `NAME`, `DESCRIPTION`, `AUTO_EXECUTE`, and `TEMPLATE_DATA`, which are described in more detail in the table below. Attempting to update other fields returned by [bizproc.workflow.template.list](./bizproc-workflow-template-list.md) will not cause errors, but they will not be updated either. | ||
|#

## FIELDS Parameter

#|
|| **Parameter** | **Description** ||
|| **NAME**
[`string`](../data-types.md) | Name of the template ||
|| **DESCRIPTION**
[`string`](../data-types.md) | Description of the template. ||
|| **TEMPLATE_DATA**
[`file`](../data-types.md) | Content of the business process template file *.bpt. See [{#T}](../how-to-call-rest-api/how-to-upload-files.md) ||
|| **AUTO_EXECUTE**
[`integer`](../data-types.md) | Auto-execute flag, which can be:

- `0` (no auto-execute),
- `1` (execute on creation),
- `2` (execute on modification),
- `3` (execute on both creation and modification) ||
|#

## Examples

```javascript
function renameTemplate(id, name)
{
	BX24.callMethod(
		'bizproc.workflow.template.update',
		{ID: id, FIELDS: {'NAME': name}},
		function(result)
		{
			if(result.error())
				alert("Error: " + result.error());
			console.log(result);
		}
	);
}
```

{% include [Footnote on examples](../../_includes/examples.md) %}