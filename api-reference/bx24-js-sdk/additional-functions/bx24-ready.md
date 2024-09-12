# Set Up the Event Handler for Document DOM Structure Readiness BX24.ready

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- missing parameters or fields
- missing examples
- missing success response
- missing error response
- links to pages that have not yet been created (jQuery.ready or BX.ready) are not specified

{% endnote %}

{% endif %}

```js
void BX24.ready(Function handler)
```

The function `BX24.ready` sets up an event handler for the "Document DOM structure is ready for use" event. It works similarly to jQuery.ready or BX.ready.