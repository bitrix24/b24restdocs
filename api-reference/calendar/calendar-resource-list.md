# Get a list of all resources calendar.resource.list

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- examples are missing
- response in case of error is missing

{% endnote %}

{% endif %}

> Scope: [`calendar`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `calendar.resource.list` returns a list (array) of all resources.

Without parameters.

## Example

{% list tabs %}

- JS

    ```js
    BX24.callMethod("calendar.resource.list")
    ```

{% endlist %}

{% include [Footnote about examples](../../_includes/examples.md) %}

## Successful response

Returns an array, each element of which has the fields "ID", "NAME", "CREATED_BY".