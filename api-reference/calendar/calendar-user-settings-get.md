# Get User Calendar Settings calendar.user.settings.get

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`calendar`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `calendar.user.settings.get` returns user calendar settings.

No parameters.

## Example

{% list tabs %}

- JS

    ```js
    BX24.callMethod("calendar.user.settings.get", {});
    ```

{% endlist %}

{% include [Footnote about examples](../../_includes/examples.md) %}