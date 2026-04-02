# Scroll the Parent Window BX24.scrollParentWindow

The method `BX24.scrollParentWindow` sends a command to scroll the parent window to a specified vertical position. Starting from version `25.800.0` of the `rest` module, this method can be used in [embedding locations](../../../api-reference/widgets/index.md) of the application. The scroll will work if the application is not opened in a slider.

```js
void BX24.scrollParentWindow(Integer scroll[, Function callback])
```

## Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **scroll***
`integer` | The vertical position to scroll the parent window to. Inside the method, the value is parsed using `parseInt`, and the command is sent only if the result is not `NaN` ||
|| **callback**
`function` | A callback function that is executed after the scroll command is sent ||
|#

## Code Example

```js
BX24.init(function () {
    BX24.scrollParentWindow(0, function () {
        console.log('Scroll command sent');
    });
});
```

{% include [Note on examples](../../../_includes/examples.md) %}

## Handling the Response

The method does not return any data (`void`).

## Continue Your Learning

- [{#T}](./bx24-reload-window.md)
- [{#T}](./bx24-set-title.md)