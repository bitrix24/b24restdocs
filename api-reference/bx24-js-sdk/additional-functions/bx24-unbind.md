# Disable the function as an event handler BX24.unbind

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- missing parameters or fields
- missing examples
- missing success response
- missing error response
- links to pages that have not yet been created (BX.unbind) are not specified

{% endnote %}

{% endif %}

```js
void BX24.unbind(DOMNode element, String eventName, Function func)
```

The method `BX24.unbind` removes the function `func` as the event handler for the event `eventName` of the object `element`. It is similar to BX.unbind.