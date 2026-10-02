# Set Up the DOM Structure Readiness Handler BX24.ready

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

```js
BX24.ready(handler: callable): void;
```

The `BX24.ready` method adds a handler function that runs when the document DOM structure is ready: the browser has parsed the application page, and its elements are accessible to the script. The method is similar in purpose to `jQuery.ready`.

Page readiness and library readiness are different events. `BX24.ready` waits only for the application page, while the data from Bitrix24 may not have arrived yet by that time. If the handler needs Bitrix24 data or has to call Bitrix24 methods, use [BX24.init](../system-functions/bx24-init.md).

The method works on an application page where the [BX24.js library](../index.md) is connected and does not call Bitrix24. The method requires no scope of its own.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#| 
|| **Name** 
`type` | **Description** ||
|| **handler*** 
[`callable`](../../../api-reference/data-types.md) | The handler function. It is called without parameters when the document DOM structure is ready ||
|#

## Code Example

{% include [Note on examples](../../../_includes/examples.md) %}

```html
<p id="status">Loading...</p>

<script>
    BX24.ready(function () {
        document.getElementById('status').textContent = 'Page loaded';
    });
</script>
```

## Response Handling

The method does not return data (`void`). If the DOM structure is already ready at the time of the call, the handler still does not run immediately — it runs after the current code finishes. That is why the lines that follow `BX24.ready` run before the handler.

If the library is loaded programmatically rather than through a `<script>` tag in the markup, the handler may run later — only once all the images and styles of the page have loaded.

## Error Handling

The method does not return error codes.

#|
|| **Situation** | **What Happens** | **What to Do** ||
|| Something other than a function is passed in `handler` | Nothing happens, and there is no error | Pass a function ||
|| The handler reads Bitrix24 data, for example, the language via [BX24.getLang](./bx24-get-lang.md) | The data may not be available yet. In that case, [BX24.getLang](./bx24-get-lang.md) returns an empty string | Move the code into the [BX24.init](../system-functions/bx24-init.md) handler ||
|#

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./bx24-is-ready.md)
- [{#T}](../system-functions/bx24-init.md)