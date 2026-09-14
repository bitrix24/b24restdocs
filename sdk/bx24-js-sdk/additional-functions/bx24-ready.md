# Set Up the DOM Structure Readiness Handler BX24.ready

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

The `BX24.ready` method adds a handler function that executes once the DOM structure of the document is ready. This means the handler will run when the page has been parsed by the browser and its elements are accessible to the script. The method works similarly to `jQuery.ready` or `BX.ready`. If a non-function parameter is passed, the call will be ignored.

```js
void BX24.ready(Function handler)
```

## Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#| 
|| **Name** 
`type` | **Description** ||
|| **handler*** 
`function` | The handler function that will be called once the DOM structure of the document is ready ||
|#

## Code Example

{% include [Note on examples](../../../_includes/examples.md) %}

```js
BX24.ready(function () {
    console.log('DOM is ready');
});
```

## Response Handling

The method does not return data (`void`).

## Continue Learning

- [{#T}](./bx24-is-ready.md)
- [{#T}](../system-functions/bx24-init.md)