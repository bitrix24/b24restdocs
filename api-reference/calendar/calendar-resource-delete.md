# Delete Resource calendar.resource.delete

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- parameter types are not specified
- examples are missing
- response in case of error is absent

{% endnote %}

{% endif %}

> Scope: [`calendar`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `calendar.resource.delete` removes a resource.

#|
|| **Parameter** | **Description** ||
|| **resourceId**^*^ | Resource identifier. ||
|#

{% include [Parameter Note](../../_includes/required.md) %}

## Example

```js
BX24.callMethod("calendar.resource.delete",
    {
        resourceId: 521
    }
);
```

{% include [Example Note](../../_includes/examples.md) %}

## Response on Success

Returns true if the deletion is successful.