# Add New Resource calendar.resource.add

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- examples are missing
- no response in case of error

{% endnote %}

{% endif %}

> Scope: [`calendar`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `calendar.resource.add` adds a new resource and takes an array of parameters as input.

#|
|| **Parameter** / **Type** | **Description** ||
|| **name**^*^ 
[`string`](../data-types.md) | Name of the resource. ||
|#

{% include [Parameter Note](../../_includes/required.md) %}

## Example

```js
BX24.callMethod("calendar.resource.add",
    {
        name: 'My resource title'
    }
);
```

{% include [Example Note](../../_includes/examples.md) %}

## Response on Success

Returns the ID of the added resource.