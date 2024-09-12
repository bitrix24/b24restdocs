# Get a List of Available Methods method.get

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- required parameters are not specified
- examples are missing
- success response is absent
- error response is absent

{% endnote %}

{% endif %}

> Scope: [`basic`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The `method.get` method returns 2 parameters:

- `isExisting => true/false` - this parameter indicates whether the method exists on this account;
- `isAvailable => true/false` - this parameter indicates whether the method is available for invocation with the current access permissions ([scope](./scope.md)) of the application.

## Parameters
#|
|| **Parameter** | **Description** ||
|| **name**
[`string`](../../data-types.md) | The name of the method to check. ||
|#

## Examples

```js
$result = CRest::call(
    'method.get',
    [
        'name' => 'user.get',
    ]
);
print_r($result);
```

{% include [Note on examples](../../../_includes/examples.md) %}