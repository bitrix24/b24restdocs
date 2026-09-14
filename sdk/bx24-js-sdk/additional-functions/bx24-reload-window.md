# Reload the Page BX24.reloadWindow

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

The method `BX24.reloadWindow` sends a command to reload the page with the application. Starting from version `25.800.0` of the `rest` module, this method can be used in [embedding locations](../../../api-reference/widgets/index.md) of the application.

```js
void BX24.reloadWindow([Function callback])
```

## Parameters

#| 
|| **Name**
`type` | **Description** ||
|| **callback**
`function` | Callback function that is executed after the command to reload the page is sent ||
|#

## Code Example

{% include [Example Footnote](../../../_includes/examples.md) %}

```js
BX24.init(function () {
    BX24.reloadWindow(function () {
        console.log('The command to reload the page has been sent');
    });
});
```

## Response Handling

The method does not return any data (`void`).

## Continue Learning

- [{#T}](./bx24-fit-window.md)
- [{#T}](./bx24-resize-window.md)