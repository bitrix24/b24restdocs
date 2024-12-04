# Update Business Process Template bizproc.workflow.template.update

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for standard writing
- parameter types not specified
- parameter requirements not specified
- examples missing
- success response missing
- error response missing

{% endnote %}

{% endif %}

> Scope: [`bizproc`](../scopes/permissions.md)
>
> Who can execute the method: administrator

This method only updates templates that were created using the [bizproc.workflow.template.add](./bizproc-workflow-template-add.md) method, as such templates are tied to the application and only they can be updated.

#|
|| **Parameter** | **Description** | **Available since** ||
|| **ID** | Identifier of the template to be modified. | ||
|| **FIELDS** | Array of modifiable parameters. The fields that can be updated are: `NAME`, `DESCRIPTION`, `AUTO_EXECUTE`, and `TEMPLATE_DATA`, described in more detail in the table below. Attempting to update other fields returned by [bizproc.workflow.template.list](./bizproc-workflow-template-list.md) will not cause errors, but they will not be updated either. | ||
|#

## FIELDS Parameter

#|
|| **Parameter** | **Description** ||
|| **NAME**
[`string`](../data-types.md) | Template name ||
|| **DESCRIPTION**
[`string`](../data-types.md) | Template description. ||
|| **TEMPLATE_DATA**
[`file`](../data-types.md) | Content of the business process template file *.bpt. See [{#T}](../how-to-call-rest-api/how-to-upload-files.md) ||
|| **AUTO_EXECUTE**
[`integer`](../data-types.md) | Auto-execute flag, can be:

- `0` (no auto-execute),
- `1` (execute on creation),
- `2` (execute on modification),
- `3` (execute on both creation and modification) ||
|#

## Examples

{% list tabs %}

- JS

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

{% endlist %}

{% include [Examples note](../../_includes/examples.md) %}