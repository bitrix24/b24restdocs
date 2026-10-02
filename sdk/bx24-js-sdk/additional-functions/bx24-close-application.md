# Close the Window with the Application BX24.closeApplication

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

```js
BX24.closeApplication(callback?: callable): void;
```

The `BX24.closeApplication` method closes the window in which Bitrix24 displays the application on top of the page. This window slides out from the right, like a slider, and it is opened by the [BX24.openApplication](./bx24-open-application.md) method. The method is also useful in embedding locations that open the application in a slider, for example, in a [CRM list menu item](../../../api-reference/widgets/crm/list-menu.md). In the window, you can add a button that closes it.

The application that opened the window can close it too: it calls `BX24.closeApplication` in its own frame as well.

The method works only inside the application frame in Bitrix24. Call it after the library is initialized, in the [BX24.init](../system-functions/bx24-init.md) handler. The method requires no scope of its own: it controls the interface and does not call the REST API.

## Method Parameters

#| 
|| **Name** 
`type` | **Description** ||
|| **callback** 
[`callable`](../../../api-reference/data-types.md) | The library accepts the parameter, but Bitrix24 does not call this function. The [Response Handling](#response) section describes how to find out that the window has closed ||
|#

## Code Example

{% include [Example Footnote](../../../_includes/examples.md) %}

Close the window on a button click:

```html
<button id="close-window">Close</button>

<script>
    BX24.init(function () {
        document.getElementById('close-window').addEventListener('click', function () {
            BX24.closeApplication();
        });
    });
</script>
```

For an example in which the application opens itself in a window and closes it, see the [BX24.openApplication](./bx24-open-application.md) page.

## Response Handling {#response}

The method does not return data (`void`). The application that opened the window can find out that it has closed: Bitrix24 calls the `closeCallback` function passed to [BX24.openApplication](./bx24-open-application.md). The function receives one parameter — an empty array `[]`.

## Error Handling

The method does not return error codes.

#|
|| **Situation** | **What Happens** | **What to Do** ||
|| The application window is not open: the application runs on its own page in Bitrix24 and has not opened a window | Nothing happens | Show the close button only when the application is open in a window. For example, pass a window flag in the `params` parameter of the [BX24.openApplication](./bx24-open-application.md#params) method. The application in the window reads it from the `PLACEMENT_OPTIONS` request parameter ||
|| Another slider is open on top of the application window, for example, a page from [BX24.openPath](./bx24-open-path.md) | Nothing happens: the method closes only the top slider and only if it is the application window | Wait until the user closes the top slider ||
|| The code waits for `callback` to be called after the window closes | The function is not called | Run the required code in `closeCallback` of the [BX24.openApplication](./bx24-open-application.md) method ||
|#

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./bx24-open-application.md)
- [{#T}](./bx24-open-path.md)