# Get Role by ID documentgenerator.role.get

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

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

The method `documentgenerator.role.get` returns information about the role and its access permissions.

#|
|| **Parameter** | **Description** ||
|| **id**^*^ | Role identifier. ||
|#

{% include [Parameter Notes](../../../_includes/required.md) %}

## Successful Response

> 200 OK

```json
"role": {
	"id": "1",
	"name": "Administrator",
	"code": "ADMIN",
	"permissions": {
		"settings": {
			"modify" : "X",
		},
		"templates": {
			"modify" : "X",
		},
		"documents": {
			"modify" : "X",
			"view" : "X",
		},
	}
}
```