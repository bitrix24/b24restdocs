# Adjusting the Frame Size to Fit Content with BX24.fitWindow

The `BX24.fitWindow` method sends a command to change the height of the application frame based on the height of the page content. It utilizes `BX24.getScrollSize().scrollHeight` in its implementation.

```js
void BX24.fitWindow([Function callback])
```

## Parameters

#|
|| **Name**
`type` | **Description** ||
|| **callback**
`function` | The callback function that executes after the resize command is sent ||

|#

## Code Example

```js
BX24.init(function () {
    BX24.fitWindow(function () {
        console.log('The command to resize the frame has been sent');
    });
});
```

{% include [Footnote on examples](../../../_includes/examples.md) %}

## Handling the Response

The method does not return any data (`void`).

## Continue Your Learning

- [{#T}](./bx24-resize-window.md)
- [{#T}](./bx24-get-scroll-size.md)