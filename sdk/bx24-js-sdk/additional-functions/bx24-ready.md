# Set Up the DOM Structure Readiness Handler BX24.ready

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../mcp.md) so that the assistant can utilize the official REST documentation.

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