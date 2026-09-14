# Scroll Parent Window BX24.scrollParentWindow

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

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
`integer` | The vertical position to scroll the parent window. Inside the method, the value is converted using `parseInt`, and the command is sent only if the result is not `NaN` ||
|| **callback** 
`function` | The callback function that is executed after the scroll command is sent ||
|#

## Code Example

{% include [Footnote on examples](../../../_includes/examples.md) %}

```js
BX24.init(function () {
    BX24.scrollParentWindow(0, function () {
        console.log('Scroll command sent');
    });
});
```

## Response Handling

The method does not return any data (`void`).

## Continue Learning

- [{#T}](./bx24-reload-window.md)
- [{#T}](./bx24-set-title.md)