# Change Frame Size with BX24.resizeWindow

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

```js
BX24.resizeWindow(width: integer | string, height: integer | string, callback?: callable): void;
```

The `BX24.resizeWindow` method sets the width and height of the frame in which Bitrix24 displays the application.

The method works only inside the application frame in Bitrix24. Call it after the library is initialized, in the [BX24.init](../system-functions/bx24-init.md) handler. The method requires no scope of its own: it controls the interface and does not call the REST API.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#| 
|| **Name** 
`type` | **Description** ||
|| **width*** 
[`integer`\|`string`](../../../api-reference/data-types.md) | The frame width in pixels, greater than zero. You can also pass a string that starts with a number, for example `'980px'` ||
|| **height*** 
[`integer`\|`string`](../../../api-reference/data-types.md) | The frame height in pixels, greater than zero. The library reads a string the same way as for `width` ||
|| **callback** 
[`callable`](../../../api-reference/data-types.md) | Callback function. It receives an object with the new frame size [(detailed description)](#callback) ||
|#

{% note warning "" %}

The string `'100%'` does not stretch the frame: the library reads it as 100 pixels. To stretch the frame to the full width and fit the height to the content, use [BX24.fitWindow](./bx24-fit-window.md).

{% endnote %}

## Code Example

{% include [Note on examples](../../../_includes/examples.md) %}

```js
BX24.init(function () {
    BX24.resizeWindow(980, 700, function (size) {
        console.log('New frame size:', size.width, size.height);
    });
});
```

## Response Handling {#callback}

The method does not return data (`void`). When Bitrix24 resizes the frame, it calls `callback` and passes an object with the width and height it has set:

```json
{
    "width": 980,
    "height": 700
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **width**
[`integer`](../../../api-reference/data-types.md) | The frame width in pixels ||
|| **height**
[`integer`](../../../api-reference/data-types.md) | The frame height in pixels. This is the height that Bitrix24 has set, not the one visible on the screen — see the warning below for details ||
|#

{% note warning "" %}

When the application is open on its own page in Bitrix24, the frame on the screen is never less than 600 pixels high. If you pass a smaller height, `callback` receives it, but the frame remains 600 pixels high.

{% endnote %}

## Error Handling

The method does not return error codes. If Bitrix24 does not execute the command, `callback` is not called.

#|
|| **Situation** | **What Happens** | **What to Do** ||
|| The width or height does not yield a positive number: `0`, a negative number, or a string such as `'auto'` is passed | The frame size does not change, and `callback` is not called | Pass both sizes as positive numbers ||
|| The method is called in the window opened by [BX24.openApplication](./bx24-open-application.md) | The window size does not change, and `callback` is not called | Do not change the size from inside the window. Bitrix24 sets the window height, and the width can be passed with the `width` parameter in `settings` when calling [BX24.openApplication](./bx24-open-application.md#settings) ||
|#

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./bx24-fit-window.md)
- [{#T}](./bx24-get-scroll-size.md)