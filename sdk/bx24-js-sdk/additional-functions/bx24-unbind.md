# Disable Function as Event Handler BX24.unbind

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

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