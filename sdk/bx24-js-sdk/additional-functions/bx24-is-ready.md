# Check the Readiness of the DOM Structure with BX24.isReady

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../mcp.md) so that the assistant can utilize the official REST documentation.

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