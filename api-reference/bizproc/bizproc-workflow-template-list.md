# Get the list of templates bizproc.workflow.template.list

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for standard writing
- missing parameters or fields
- required parameters not specified
- examples missing
- success response missing
- error response missing

{% endnote %}

{% endif %}

> Scope: [`bizproc`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method returns a list of Business Process templates added to Bitrix24.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **ID** | Identifier of the business process. ||
|| **MODULE_ID** | Module identifier (by document) ||
|| **ENTITY** | Entity identifier (by document) ||
|| **DOCUMENT_TYPE** | Document type ||
|| **AUTO_EXECUTE** | Auto-execute flag. Can take values:

- `0` (no auto-execute),
- `1` (execute on creation),
- `2` (execute on modification),
- `3` (execute on both creation and modification)
||
|| **NAME** | Template name ||
|| **TEMPLATE** | BP template (array with action structure description). ||
|| **PARAMETERS** | Template parameters, array with property descriptions. ||
|| **VARIABLES** | Template variables, array with property descriptions. ||
|| **CONSTANTS** | Template constants, array with property descriptions. ||
|| **MODIFIED** | Date of last modification. ||
|| **IS_MODIFIED** | [Y\N] Flag indicating whether it has been modified. Relevant for templates supplied "off-the-shelf" (which have a system code) ||
|| **USER_ID** | Identifier of the user who created/modified the template. ||
|| **SYSTEM_CODE** | System code of the template. Used for identifying standard templates, processes in the feed, automation templates, etc. ||
|| **START** | The ordinal number of the list item from which to return the next items when calling the current method. Details in the article [{#T}](../how-to-call-rest-api/list-methods-pecularities.md) ||
|#

## Examples

{% list tabs %}

- JS
	
	```javascript
	function getTemplates()
	{
		BX24.callMethod(
			'bizproc.workflow.template.list',
			{
				select: [
					'ID',
					// 'MODULE_ID',
					// 'ENTITY',
					// 'DOCUMENT_TYPE',
					// 'AUTO_EXECUTE',
					'NAME',
					// 'TEMPLATE',
					// 'PARAMETERS',
					// 'VARIABLES',
					// 'CONSTANTS',
					'MODIFIED',
					'IS_MODIFIED',
					'USER_ID',
					'SYSTEM_CODE'
				],
				filter: {MODULE_ID: 'crm', ENTITY: 'CCrmDocumentLead'}
			},
			function(result)
			{
				if(result.error())
					alert("Error: " + result.error());
				else
				{
					var templates = result.data();
					console.log(templates);
				}
			}
		);
	}
	```

- B24-PHP-SDK

	```php
	try {
		$result = $serviceBuilder
			->getBizProcScope()
			->template()
			->list(
				['ID', 'MODULE_ID', 'ENTITY', 'DOCUMENT_TYPE', 'AUTO_EXECUTE', 'NAME', 'TEMPLATE', 'PARAMETERS', 'VARIABLES', 'CONSTANTS', 'MODIFIED', 'IS_MODIFIED', 'USER_ID', 'SYSTEM_CODE'],
				[]
			);

		foreach ($result->getTemplates() as $template) {
			print("ID: " . $template->ID . "\n");
			print("MODULE_ID: " . $template->MODULE_ID . "\n");
			print("ENTITY: " . $template->ENTITY . "\n");
			print("DOCUMENT_TYPE: " . json_encode($template->DOCUMENT_TYPE) . "\n");
			print("AUTO_EXECUTE: " . ($template->AUTO_EXECUTE ? $template->AUTO_EXECUTE->value : 'null') . "\n");
			print("NAME: " . $template->NAME . "\n");
			print("TEMPLATE: " . json_encode($template->TEMPLATE) . "\n");
			print("PARAMETERS: " . json_encode($template->PARAMETERS) . "\n");
			print("VARIABLES: " . json_encode($template->VARIABLES) . "\n");
			print("CONSTANTS: " . json_encode($template->CONSTANTS) . "\n");
			print("MODIFIED: " . ($template->MODIFIED ? $template->MODIFIED->format(DATE_ATOM) : 'null') . "\n");
			print("IS_MODIFIED: " . ($template->IS_MODIFIED ? 'true' : 'false') . "\n");
			print("USER_ID: " . $template->USER_ID . "\n");
			print("SYSTEM_CODE: " . $template->SYSTEM_CODE . "\n");
			print("\n");
		}
	} catch (Throwable $e) {
		print("Error: " . $e->getMessage() . "\n");
	}
	```
		
{% endlist %}

{% include [Note about examples](../../_includes/examples.md) %}