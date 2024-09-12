# Run the javascript file BX24.loadScript

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- missing parameters or fields
- missing examples
- missing success response
- missing error response

{% endnote %}

{% endif %}

```js
void BX24.loadScript(Array|String script[, Function callback])
```

The method `BX24.loadScript` loads and executes the client-side javascript file `script`. You can pass an array of files, in which case they will be loaded sequentially. The `callback` handler will be called after all loads are complete.