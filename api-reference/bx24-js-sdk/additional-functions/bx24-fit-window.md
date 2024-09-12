# Adjust Frame Size to Content BX24.fitWindow

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
void BX24.fitWindow([Function callback])
```

The method `BX24.fitWindow` adjusts the size of the application frame according to the dimensions of the frame's content.

Due to browser limitations, the method can only increase the frame size. To set the size accurately, you can use the following approach:

- wrap the application content in a container;
- calculate the dimensions of the container;
- set the frame size based on the container's dimensions using [BX24.resizeWindow](./bx24-resize-window.md).

## See also:

- [BX24.resizeWindow](./bx24-resize-window.md)
- [BX24.getScrollSize](./bx24-get-scroll-size.md)