# Set the Page Heading BX24.setTitle

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

The `BX24.setTitle` method sends a command to change the application page heading.

```js
void BX24.setTitle(String title[, Function callback])
```

The `BX24.setTitle` method changes the heading within the application container: a page or a slider. The portal page controls the heading of the top browser tab, so the command may execute successfully, but the browser tab heading will not change.

## Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **title***
`string` | New page title. Inside the method, the value is converted to a string via `toString()` ||
|| **callback**
`function` | Callback function, executed after sending the command to change the title ||
|#

## Code Example

{% include [Note on examples](../../../_includes/examples.md) %}

```js
BX24.init(function () {
    BX24.setTitle('New headline', function () {
        console.log('Command to change the headline has been sent');
    });
});
```

## Response Handling

The method does not return data (`void`).

## Continue Learning

- [{#T}](./bx24-scroll-parent-window.md)
- [{#T}](./bx24-reload-window.md)