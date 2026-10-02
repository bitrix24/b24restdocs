# Get Application Page Dimensions with BX24.getScrollSize

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

```js
BX24.getScrollSize(): object;
```

The `BX24.getScrollSize` method returns the width and height of the application page inside the frame. [BX24.fitWindow](./bx24-fit-window.md) uses this height when it fits the frame to the content.

The dimensions are never smaller than the visible part of the frame: if the content is shorter than the frame, the method returns the frame height. To find out the height of the content itself, measure the element that contains it, for example, using its `offsetHeight` property.

The method measures the application page and does not call Bitrix24, so it also works before [BX24.init](../system-functions/bx24-init.md). The method requires no scope of its own.

## Method Parameters

No parameters.

## Code Example

{% include [Example Footnote](../../../_includes/examples.md) %}

```js
BX24.init(function () {
    const size = BX24.getScrollSize();
    console.log(size.scrollWidth, size.scrollHeight); // 1108 604
});
```

## Response Handling

The method synchronously returns an object with the dimensions of the application page:

```json
{
    "scrollWidth": 1108,
    "scrollHeight": 604
}
```

### Returned Data

#| 
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../api-reference/data-types.md) | An object with the dimensions of the application page [(detailed description)](#result) ||
|#

### Object result {#result}

#| 
|| **Name**
`type` | **Description** ||
|| **scrollWidth**
[`integer`](../../../api-reference/data-types.md) | The width of the application page in pixels, but not less than the frame width ||
|| **scrollHeight**
[`integer`](../../../api-reference/data-types.md) | The height of the application page in pixels, but not less than the frame height ||
|#

## Error Handling

The method does not return error codes.

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./bx24-fit-window.md)
- [{#T}](./bx24-resize-window.md)