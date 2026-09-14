# Get Frame Dimensions with BX24.getScrollSize

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

The method `BX24.getScrollSize` returns the dimensions of the current frame's content as an object with the fields `scrollWidth` and `scrollHeight`.

```js
Object BX24.getScrollSize()
```

## Parameters

No parameters.

## Code Example

{% include [Example Footnote](../../../_includes/examples.md) %}

```js
BX24.init(function () {
    const size = BX24.getScrollSize();
    console.log(size.scrollWidth, size.scrollHeight);
});
```

## Response Handling

The method synchronously returns an object containing the dimensions of the content.

### Returned Data

#| 
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../api-reference/data-types.md) | An object with the dimensions of the frame's content [(detailed description)](#result) ||
|#

### Object result {#result}

#| 
|| **Name**
`type` | **Description** ||
|| **scrollWidth**
[`integer`](../../../api-reference/data-types.md) | The width of the frame's content in pixels ||
|| **scrollHeight**
[`integer`](../../../api-reference/data-types.md) | The height of the frame's content in pixels ||
|#

## Continue Learning

- [{#T}](./bx24-resize-window.md)
- [{#T}](./bx24-fit-window.md)