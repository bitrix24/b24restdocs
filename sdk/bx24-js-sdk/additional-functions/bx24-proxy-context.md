# Get the Execution Context of the Proxy Function BX24.proxyContext

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The method `BX24.proxyContext` returns the original context of the call within a proxy function created through [BX24.proxy](./bx24-proxy.md). Outside of such a call, the method returns `null`.

```js
Object BX24.proxyContext()
```

## Parameters

No parameters.

## Code Example

{% include [Example Footnote](../../../_includes/examples.md) %}

```js
BX24.init(function () {
    const context = {
        onClick: function () {
            console.log(BX24.proxyContext());
        }
    };

    const button = document.getElementById('run-action');
    BX24.bind(button, 'click', BX24.proxy(context.onClick, context));
});
```

## Response Handling

The method synchronously returns a result of type `object`.

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../api-reference/data-types.md) | The original context of the proxy function call. If the method is called outside of a proxy function, it returns `null` ||
|#

## Continue Learning

- [{#T}](./bx24-proxy.md)
- [{#T}](./bx24-bind.md)