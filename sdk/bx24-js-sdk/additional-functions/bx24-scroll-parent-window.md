# Scroll Parent Window BX24.scrollParentWindow

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

```js
BX24.scrollParentWindow(scroll: integer | string, callback?: callable): void;
```

The `BX24.scrollParentWindow` method scrolls the parent window — the Bitrix24 page that contains the application frame — to the specified vertical position. For example, if the application frame is taller than the screen, a "Back to top" button at the bottom of the application can return the user to the top of the page.

Starting from version `25.800.0` of the `rest` module, this method can be used in [embedding locations](../../../api-reference/widgets/index.md) of the application. Scrolling works if the application is not open in a slider, for example, on its own page in Bitrix24. In the window opened by [BX24.openApplication](./bx24-open-application.md), the page does not scroll.

The method works only inside the application frame in Bitrix24. Call it after the library is initialized, in the [BX24.init](../system-functions/bx24-init.md) handler. The method requires no scope of its own: it controls the interface and does not call the REST API.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#| 
|| **Name** 
`type` | **Description** ||
|| **scroll*** 
[`integer`\|`string`](../../../api-reference/data-types.md) | The position in pixels from the top of the Bitrix24 page. `0` is the top of the page. If the number is greater than the page height, Bitrix24 scrolls the page to the bottom. You can also pass a string that starts with a number: the library takes `150` from `'150px'` ||
|| **callback** 
[`callable`](../../../api-reference/data-types.md) | Callback function. It receives an object with the passed position [(detailed description)](#callback) ||
|#

## Code Example

{% include [Footnote on examples](../../../_includes/examples.md) %}

Scroll the Bitrix24 page to the top on a button click:

```html
<button id="scroll-top">Back to top</button>

<script>
    BX24.init(function () {
        document.getElementById('scroll-top').addEventListener('click', function () {
            BX24.scrollParentWindow(0, function (result) {
                console.log('Position:', result.scroll);
            });
        });
    });
</script>
```

## Response Handling {#callback}

The method does not return data (`void`). When Bitrix24 processes the command, it calls `callback` and passes an object with the position you specified:

```json
{
    "scroll": 0
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **scroll**
[`integer`](../../../api-reference/data-types.md) | The position from the `scroll` parameter. This is not the actual page position: `callback` is called even when the page has not scrolled ||
|#

## Error Handling

The method does not return error codes.

#|
|| **Situation** | **What Happens** | **What to Do** ||
|| `scroll` does not yield a number, for example, the string `'top'` is passed | The page does not scroll, and `callback` is not called | Pass a number ||
|| A negative number is passed in `scroll` | The page does not scroll, but `callback` is called | Pass `0` or a positive number ||
|| The application is open in a slider, for example, in the [BX24.openApplication](./bx24-open-application.md) window | The page does not scroll, but `callback` is called | Scroll the content inside the frame from the application page itself, for example, with `window.scrollTo` ||
|#

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./bx24-reload-window.md)
- [{#T}](./bx24-set-title.md)