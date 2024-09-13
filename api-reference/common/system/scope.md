# Get a List of Available Permissions Scope

{% note warning "We are still updating this page" %}

Some data may be missing here — we will add it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- missing parameters or fields
- parameter types not specified
- examples missing
- no error response provided

{% endnote %}

{% endif %}

> Scope: [`basic`](../../scopes/permissions.md)
>
> Who can execute the method: any user

If the `scope` method is called without parameters, it will return all permissions available for this application.

If the method is called with the parameter `full=true`, it will return the complete [list of permissions](../../scopes/permissions.md).

## Examples

```http
https://my.bitrix24.com/rest/scope?auth=d161f25928c3184678924ec127edd29a
```

{% include [Example Notes](../../../_includes/examples.md) %}

## Successful Response

> 200 OK
```json
{
    "result":[
        "entity",
        "user",
        "log",
        "department"
    ]
}
```