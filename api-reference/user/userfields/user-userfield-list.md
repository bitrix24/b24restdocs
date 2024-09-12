# Get a List of Custom Fields user.userfield.list

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameters or fields and their types may be needed
- the mandatory nature of parameters is not specified
- examples in other languages are missing
- response in case of success is missing
- response in case of error is missing

{% endnote %}

{% endif %}

> Scope: [`user.userfield`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method allows you to retrieve a list of custom fields. It is only available to users with administrator rights. Only filtering and sorting are available.

Filtering is available by the following keys:

- **ID**
- **FIELD_NAME**
- **USER_TYPE_ID**
- **XML_ID**
- **SORT**
- **MULTIPLE**
- **MANDATORY**
- **SHOW_FILTER**
- **SHOW_IN_LIST**
- **EDIT_IN_LIST**
- **IS_SEARCHABLE**
- **LANG**

## Example

```php
CRest::call(
    'user.userfield.list',
    [
        'order' => ['ID' => 'desc'],
        'filter' => ['ID' => 42],
    ]
);
```
{% include [Footnote on examples](../../../_includes/examples.md) %}