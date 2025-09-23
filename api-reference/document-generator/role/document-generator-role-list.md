# Get the list of roles documentgenerator.role.list

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- examples are missing
- response in case of error is absent

{% endnote %}

{% endif %}

> Scope: [`documentgenerator`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `documentgenerator.role.list` will return a list of roles without their access permissions.

#|
|| **Parameter** | **Description** ||
|| **start** | The ordinal number of the list item from which to return the next items when calling the current method. Details in the article [{#T}](../../../settings/how-to-call-rest-api/list-methods-pecularities.md) ||
|#

## Response in case of success

> 200 OK

```json
"roles": {
	[
		 "id": "1",
		 "name": "Administrator",
		 "code": "ADMIN",
	],
	[
		"id": "2",
		"name": "Manager",
		"code": "MANAGER",
	],
}
```