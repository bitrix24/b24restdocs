# Get the list of numerators documentgenerator.numerator.list

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- examples are missing
- response in case of error is absent

{% endnote %}

{% endif %}

> Scope: [`documentgenerator`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `documentgenerator.numerator.list` returns a list of numerators.

#|
|| **Parameter** | **Description** ||
|| **start** | The ordinal number of the list item from which to return the subsequent items when calling the current method. Details in the article [{#T}](../../../settings/how-to-call-rest-api/list-methods-pecularities.md) ||
|#

## Response in case of success

> 200 OK

```json
"numerators": [
	0: {
		"id": "202", // template id
		"name": "Rest Template", // name
		"template": "{NUMBER}", // template
		"settings": { // generator settings
			"Bitrix_Main_Numerator_Generator_SequentNumberGenerator": {
				"start":  20,
				"step":  5,
				"periodicBy": '',  
				"timezone": '',  
				"isDirectNumeration": ''  
			}	
		}
	}
]
```