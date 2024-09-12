# Get Information About the BX24.placement.info Call Context

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- missing parameters or fields
- parameter types not specified
- parameter requirements not indicated
- examples missing
- success response missing
- error response missing
- links to pages not yet created are not specified.

{% endnote %}

{% endif %}

Retrieving information about the call context.

Example call

```js
BX24.placement.info();
```

Result:

```json
{"placement":"CRM_LEAD_LIST_MENU","options":{"ID":"1348"}}
```

{% include [Footnote on examples](../../../_includes/examples.md) %}