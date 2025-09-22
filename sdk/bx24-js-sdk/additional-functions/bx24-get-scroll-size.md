# Get Frame Dimensions BX24.getScrollSize

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

```js
Object BX24.getScrollSize()
```

The function `BX24.getScrollSize` will return the internal dimensions of the application frame as an object with the fields **scrollWidth** and **scrollHeight**. The returned values may depend not only on the frame size but also on the application's layout.

## See also:

- [BX24.resizeWindow](./bx24-resize-window.md)
- [BX24.fitWindow](./bx24-fit-window.md)