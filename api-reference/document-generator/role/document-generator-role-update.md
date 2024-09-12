# Change Role documentgenerator.role.update

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

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

The method `documentgenerator.role.update` updates a role.

#|
|| **Parameter** | **Description** ||
|| **id**^*^ | Role ID. ||
|| **fields** | Array of fields. ||
|#

{% include [Parameter Note](../../../_includes/required.md) %}

## Fields Parameters

#|
|| **Parameter** | **Description** ||
|| **name** | Name of the country. ||
|| **code** | Role code. ||
|| **permissions** | Role permissions. This is an array of the following format: 

```json
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
``` 

The first key is the entity, the second is the action, and the value is the level of permissions. If an empty array is passed, the role will have no permissions. The following levels exist: empty value - no permission, `A` - own, `D` - own and department colleagues, `X` - all allowed.

Levels `A` and `D` are only relevant for `permissions[templates][modify]`. ||
|#

All parameters are optional.

## Success Response

Returns the same data as when calling the new role [documentgenerator.role.get()](./document-generator-role-get.md) in the new region.