# Get a List of Available Methods

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- missing parameters or fields
- parameter types not specified
- parameter requirements not specified
- examples missing
- error response missing

{% endnote %}

{% endif %}

> Scope: [`basic`](../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note alert %}

The `methods` method is deprecated and will be removed by September 1, 2022. It is strongly recommended to use the [method.get](./method-get.md) method instead.

{% endnote %}

## Parameters

No parameters - displays a list of methods available to the current application.

### Additional Parameters

#|
|| **Field** | **Description** ||
|| `full=true` | displays all methods. ||
|| `scope=permission_name` | displays methods included in this permission. If the parameter is specified without a value (`methods?scope=&auth=xxxxx`), all common methods will be displayed. ||
|#

## Examples

```http
https://my.bitrix24.com/rest/methods?scope=&auth=d161f25928c3184678924ec127edd29a - show all public methods.
```

{% include [Example Note](../../../_includes/examples.md) %}

## Successful Response

> 200 OK
```json
{
    "result":[
        "scope",
        "methods",
        "batch",
        "user.admin",
        "user.access",
        "access.name"
    ]
}
```