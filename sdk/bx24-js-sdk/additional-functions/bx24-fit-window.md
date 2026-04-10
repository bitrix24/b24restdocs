# Adjust Frame Size to Content with BX24.fitWindow

The method `BX24.fitWindow` sends a command to change the height of the application frame to match the height of the page content. It utilizes `BX24.getScrollSize().scrollHeight` in its implementation.

```js
void BX24.fitWindow([Function callback])
```

## Parameters

#| 
|| **Name** 
`type` | **Description** ||
|| **callback** 
`function` | Callback function executed after the resize command is sent ||
|#

## Code Example

{% include [Example Note](../../../_includes/examples.md) %}

```js
BX24.init(function () {
    BX24.fitWindow(function () {
        console.log('Frame resize command sent');
    });
});
```

## Response Handling

The method does not return any data (`void`).

## Continue Learning

- [{#T}](./bx24-resize-window.md)
- [{#T}](./bx24-get-scroll-size.md)