# Reload the Page BX24.reloadWindow

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

```js
BX24.reloadWindow(callback?: callable): void;
```

The `BX24.reloadWindow` method reloads the Bitrix24 page on which the application is open. The application itself reloads along with the page. Starting from version `25.800.0` of the `rest` module, this method can be used in [embedding locations](../../../api-reference/widgets/index.md) of the application.

To reload only the application and leave the Bitrix24 page as is, call `location.reload()` in the code of the application page: the library is initialized again.

The method works only inside the application frame in Bitrix24. Call it after the library is initialized, in the [BX24.init](../system-functions/bx24-init.md) handler. The method requires no scope of its own: it controls the interface and does not call the REST API.

## Method Parameters

#| 
|| **Name**
`type` | **Description** ||
|| **callback**
[`callable`](../../../api-reference/data-types.md) | The library accepts the parameter, but Bitrix24 does not call this function: the page reloads together with the application ||
|#

## Code Example

{% include [Example Footnote](../../../_includes/examples.md) %}

Reload the page on a button click:

```html
<button id="reload-page">Reload page</button>

<script>
    BX24.init(function () {
        document.getElementById('reload-page').addEventListener('click', function () {
            BX24.reloadWindow();
        });
    });
</script>
```

## Response Handling

The method does not return data (`void`). Place the code that must run after the reload in the [BX24.init](../system-functions/bx24-init.md) handler: after the reload, the application goes through initialization again.

## Error Handling

The method does not return error codes.

#|
|| **Situation** | **What Happens** | **What to Do** ||
|| The method is called immediately in the [BX24.init](../system-functions/bx24-init.md) handler | The page reloads endlessly: after each reload, the handler runs again | Call the method on a user action or under a condition that does not recur after the reload ||
|| The method is called in the window opened by [BX24.openApplication](./bx24-open-application.md) | The entire page under the window reloads, and the window closes | To reload only the application in the window, call `location.reload()` in the code of its page ||
|#

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./bx24-fit-window.md)
- [{#T}](./bx24-resize-window.md)