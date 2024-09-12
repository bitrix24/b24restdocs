# Get a list of all resources calendar.resource.list

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- examples are missing
- response in case of an error is missing

{% endnote %}

{% endif %}

> Scope: [`calendar`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `calendar.resource.list` returns a list (array) of all resources.

No parameters.

## Example

```js
BX24.callMethod("calendar.resource.list")
```

{% include [Note on examples](../../_includes/examples.md) %}

## Successful response

Returns an array, where each element has the fields "ID", "NAME", "CREATED_BY".