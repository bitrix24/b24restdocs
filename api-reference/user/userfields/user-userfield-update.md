# Update User Field user.userfield.update

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameters or fields and their types may be needed
- required parameters are not specified
- examples in other languages are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`user.userfield`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method updates a user field.

{% note info "" %}

**Attention!** MULTIPLE fields are not editable.

{% endnote %}

## Example

{% list tabs %}

- PHP

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

{% endlist %}

{% include [Footnote on examples](../../../_includes/examples.md) %}