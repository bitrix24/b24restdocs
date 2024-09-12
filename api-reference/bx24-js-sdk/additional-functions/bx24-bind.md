# Set Function as Event Handler BX24.bind

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- missing parameters or fields
- missing examples
- missing success response
- missing error response

{% endnote %}

{% endif %}

```js
void BX24.bind(DOMNode element, String eventName, Function func)
```

The method `BX24.bind` sets the function `func` as the event handler for the `eventName` of the `element` object. It is similar to `BX.bind`.