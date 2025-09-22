# Change Frame Size BX24.resizeWindow

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
void BX24.resizeWindow(Integer width, Integer height[, Function callback])
```

The method `BX24.resizeWindow` changes the size of the frame with the application, including embeddings in the form of custom fields. If the frame width exceeds the maximum allowable width of the **Bitrix24** page, part of the frame will be hidden due to layout constraints. The maximum allowable width depends on the user's screen resolution.

## See also:

- [BX24.getScrollSize](./bx24-get-scroll-size.md)
- [BX24.fitWindow](./bx24-fit-window.md)