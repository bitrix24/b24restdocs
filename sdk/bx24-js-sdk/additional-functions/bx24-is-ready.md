# Check the Readiness of the DOM Structure with BX24.isReady

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

The method `BX24.isReady` checks whether the document's DOM structure is ready for operation. This method indicates that the page has been parsed by the browser and its elements are accessible for the script.

```js
Boolean BX24.isReady()
```

## Parameters

No parameters.

## Code Example

{% include [Example Notes](../../../_includes/examples.md) %}

```js
BX24.ready(function () {
    console.log(BX24.isReady()); // true
});
```

## Response Handling

The method synchronously returns a result of type `boolean`.

### Returned Data

#|  
|| **Name**  
`type` | **Description** ||  
|| **result**  
[`boolean`](../../../api-reference/data-types.md) | `true` if the document's DOM structure is ready for operation, otherwise `false` ||  
|#

## Continue Your Learning

- [{#T}](./bx24-ready.md)  
- [{#T}](../system-functions/bx24-init.md)  