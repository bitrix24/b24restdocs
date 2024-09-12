# Update User Field user.userfield.update

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameters or fields and their types may be needed
- the requiredness of parameters is not specified
- examples in other languages are missing
- response in case of success is missing
- response in case of error is missing

{% endnote %}

{% endif %}

> Scope: [`user.userfield`](../../scopes/permissions.md)
>
> Who can execute the method: any user

This method updates a user field.

{% note info "" %}

**Attention!** MULTIPLE fields are not editable.

{% endnote %}

## Example

```php
CRest::call(
    'user.userfield.update',
    [
        'id' => 42,
        'fields' => [
            'LIST_FILTER_LABEL' => 'Title',
            'LIST_COLUMN_LABEL' => 'List Title',
        ],
    ]
);
```
{% include [Example Note](../../../_includes/examples.md) %}