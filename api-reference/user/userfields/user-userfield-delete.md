# Delete Custom Field user.userfield.delete

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- missing parameters or fields
- required parameters not specified
- missing examples in other languages
- missing success response
- missing error response

{% endnote %}

{% endif %}

> Scope: [`user.userfield`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method deletes a custom field.

{% list tabs %}

- PHP

    ```php
    CRest::call(
        'user.userfield.delete',
        [
            'id' => 42,
        ]
    );
    ```

{% endlist %}

{% include [Examples note](../../../_includes/examples.md) %}