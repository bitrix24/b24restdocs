# Delete Business Process Template bizproc.workflow.template.delete

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- missing parameters or fields
- parameter types not specified
- examples missing
- success response missing
- error response missing

{% endnote %}

{% endif %}

> Scope: [`bizproc`](../scopes/permissions.md)
>
> Who can execute the method: administrator

Deleting a Business Process Template. This method only deletes templates that were created using the [`bizproc.workflow.template.add`](./bizproc-workflow-template-add.md) method, as such templates are tied to the application and only they can be deleted.

#|
|| **Parameter** | **Description** | **Available since** ||
|| **ID**^*^ | Identifier of the template to be deleted. | |
|#	

\* - Required parameters

## Example

{% list tabs %}

- JS

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

- B24-PHP-SDK

	```php
	try {
		$templateId = 123; // Replace with the actual template ID you want to delete
		$result = $serviceBuilder
			->getBizProcScope()
			->template()
			->delete($templateId);

		if ($result->isSuccess()) {
			print("Template with ID {$templateId} deleted successfully.\n");
		} else {
			print("Failed to delete template with ID {$templateId}.\n");
		}
	} catch (\Throwable $e) {
		print("An error occurred: " . $e->getMessage() . "\n");
	}
	```

{% endlist %}


{% include [Footnote on examples](../../_includes/examples.md) %}