# Get the proxy function BX24.proxy

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- missing parameters or fields
- missing examples
- missing success response
- missing error response
- links to pages that have not yet been created (jQuery.proxy or BX.proxy) are not specified

{% endnote %}

{% endif %}

```js
Function BX24.proxy(Function func, Object thisObject)
```

The function `BX24.proxy` is similar to BX.proxy. It is analogous to jQuery.proxy, with one difference: when the function is called again with the same parameters, it will return a reference to the same proxy function that was the result of the first call, rather than a new proxy function.