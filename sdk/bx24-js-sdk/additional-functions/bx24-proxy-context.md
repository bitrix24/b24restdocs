# Get the Call Context of the Proxy Function BX24.proxyContext

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

```js
BX24.proxyContext(): any;
```

The `BX24.proxyContext` method returns the value of `this` with which a proxy function created by [BX24.proxy](./bx24-proxy.md) was called.

Inside the proxy function, `this` is replaced with the `thisObject` object, and this method returns the original value. For example, if the proxy function handles a button click, the method returns that button, not the `thisObject` object.

The value is available while the synchronous part of the proxy function is running. If you need it later, for example, after `await` or in a timer, store it in a variable at the beginning of the function.

The method works on an application page where the [BX24.js library](../index.md) is connected and does not call Bitrix24, so there is no need to wait for [BX24.init](../system-functions/bx24-init.md). The method requires no scope of its own.

## Method Parameters

No parameters.

## Code Example

{% include [Example Footnote](../../../_includes/examples.md) %}

One handler for two buttons — the method shows which of them was clicked:

```html
<button id="save">Save</button>
<button id="cancel">Cancel</button>

<script>
    BX24.ready(function () {
        const form = {
            onClick: function () {
                const button = BX24.proxyContext();
                console.log('Clicked button', button.id); // save or cancel
            }
        };

        const handler = BX24.proxy(form.onClick, form);
        BX24.bind(document.getElementById('save'), 'click', handler);
        BX24.bind(document.getElementById('cancel'), 'click', handler);
    });
</script>
```

## Response Handling

The method synchronously returns the original `this` of the current proxy function call. The type of the result depends on how the proxy function was called.

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`any`](../../../api-reference/data-types.md) | The original `this` of the proxy function call. If the proxy function is assigned as an event handler via [BX24.bind](./bx24-bind.md), the value is the element from the `element` parameter. It may differ from `event.target` if the event occurred on a nested element ||
|#

For example, if you click the "Save" button in the example above, the method returns that button element:

```html
<button id="save">Save</button>
```

## Error Handling

The method does not return error codes.

#|
|| **Situation** | **What Happens** | **What to Do** ||
|| The method is called outside a proxy function | Usually returns `null`, except after an error inside a proxy function — that case is described below | Call the method inside the function passed to [BX24.proxy](./bx24-proxy.md) ||
|| The proxy function is called directly, for example, `handler()` | Returns `undefined`: such a call has no original `this` | Assign the proxy function as an event handler or call it as an object method ||
|#

{% note warning "" %}

If a proxy function terminates with an error, the library does not restore the previous value. After that, outside proxy functions, the method may return the `this` of the interrupted call instead of `null`. Therefore, do not check the result for `null` to find out whether the code is called from a proxy function.

{% endnote %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./bx24-proxy.md)
- [{#T}](./bx24-bind.md)