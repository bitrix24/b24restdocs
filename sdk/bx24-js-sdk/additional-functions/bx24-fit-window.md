# Adjust Frame Size to Content with BX24.fitWindow

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

```js
BX24.fitWindow(callback?: callable): void;
```

The `BX24.fitWindow` method stretches the application frame to the full available width and fits its height to the page content. The method takes the height from [BX24.getScrollSize](./bx24-get-scroll-size.md).

The method works only inside the application frame in Bitrix24. Call it after the library is initialized, in the [BX24.init](../system-functions/bx24-init.md) handler, and every time the page content grows. The method requires no scope of its own: it controls the interface and does not call the REST API.

## Method Parameters

#| 
|| **Name** 
`type` | **Description** ||
|| **callback** 
[`callable`](../../../api-reference/data-types.md) | Callback function. It receives an object with the new frame size [(detailed description)](#callback) ||
|#

{% note warning "" %}

The method does not shrink the frame. If the page content becomes shorter, the frame keeps its previous height: [BX24.getScrollSize](./bx24-get-scroll-size.md) does not return a height smaller than the frame's visible height. To shrink the frame, pass the required height to [BX24.resizeWindow](./bx24-resize-window.md).

{% endnote %}

## Code Example

{% include [Example Note](../../../_includes/examples.md) %}

```js
BX24.init(function () {
    BX24.fitWindow(function (size) {
        console.log('New frame size:', size.width, size.height);
    });
});
```

## Response Handling {#callback}

The method does not return data (`void`). When Bitrix24 resizes the frame, it calls `callback` and passes an object with the new width and height:

```json
{
    "width": 1108,
    "height": 1579
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **width**
[`integer`](../../../api-reference/data-types.md) | The frame width in pixels. It equals the width of the area in which Bitrix24 displays the application ||
|| **height**
[`integer`](../../../api-reference/data-types.md) | The frame height in pixels ||
|#

## Error Handling

The method does not return error codes. If Bitrix24 does not execute the command, `callback` is not called. This happens in the window opened by [BX24.openApplication](./bx24-open-application.md): Bitrix24 sets the height of such a window, and content longer than the window scrolls inside it. The window width can be passed with the `width` parameter in `settings` when calling [BX24.openApplication](./bx24-open-application.md#settings).

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./bx24-resize-window.md)
- [{#T}](./bx24-get-scroll-size.md)