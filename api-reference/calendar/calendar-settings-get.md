# Get Main Calendar Settings calendar.settings.get

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`calendar`](../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `calendar.settings.get` returns the main settings of the calendar. It pertains to module settings, accessible only to administrators.

No parameters.

## Example

```js
BX24.callMethod("calendar.settings.get", {});
```

{% include [Footnote on examples](../../_includes/examples.md) %}