# Bind Users to Roles documentgenerator.role.fillaccesses

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- examples are missing
- success response is absent
- error response is absent

{% endnote %}

{% endif %}

> Scope: [`documentgenerator`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `documentgenerator.role.fillaccesses` will set a set of roles and their bindings.

{% note warning %}

This method does not update existing bindings. It always overwrites the entire map.

{% endnote %}

#|
|| **Parameter** | **Description** ||
|| **accesses** | Array of role bindings.
- `accesses[roleId]` - role identifier.
- `accesses[accessCode]` - symbolic code for binding. ||
|#

## Example

```json
"accesses": [
	{
		"roleId": "1",
		"accessCode" : "U1", // bind to user with ID 1
	},
	{
		"roleId": "2",
		"accessCode" : "D1", // bind to department with ID 1
	}
]
```

{% include [Footnote on examples](../../../_includes/examples.md) %}