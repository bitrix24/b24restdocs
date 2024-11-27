# Get main calendar settings calendar.settings.get

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

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
> Who can execute the method: administrator

The method `calendar.settings.get` returns the main calendar settings. It pertains to module settings, accessible only to administrators.

No parameters.

## Example

{% list tabs %}

- JS

    ```js
    BX24.callMethod("calendar.settings.get", {});
    ```

{% endlist %}

{% include [Footnote about examples](../../_includes/examples.md) %}