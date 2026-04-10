# Disable Function as Event Handler BX24.unbind

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The method `BX24.unbind` removes the function `func` from the event handlers for `eventName` on the page element `element`.

```js
void BX24.unbind(DOMNode element, String eventName, Function func)
```

## Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#| 
|| **Name** 
`type` | **Description** ||
|| **element*** 
`DOMNode` | The HTML element on the page (DOM element) for which the handler needs to be removed ||
|| **eventName*** 
`string` | The name of the event. For `mousewheel`, the `DOMMouseScroll` handler is also removed ||
|| **func*** 
`function` | The handler function to be removed ||
|#

## Code Example

{% include [Note on examples](../../../_includes/examples.md) %}

```js
BX24.init(function () {
    const button = document.getElementById('run-action');

    function onClick() {
        console.log('Button clicked');
    }

    BX24.bind(button, 'click', onClick);
    BX24.unbind(button, 'click', onClick);
});
```

## Response Handling

The method does not return data (`void`).

## Continue Learning

- [{#T}](./bx24-bind.md)
- [{#T}](./bx24-ready.md)