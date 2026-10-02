# Get the BX24.proxy Function

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

```js
BX24.proxy(func: callable, thisObject: object): callable;
```

The `BX24.proxy` method creates a proxy function. The proxy function calls `func`, and inside `func`, `this` refers to the `thisObject` object. This is needed when an object method is assigned as an event handler: without a proxy, `this` in the handler refers to the page element, not to the object.

The method is similar in purpose to `Function.prototype.bind`, but for the same pair of `func` and `thisObject` it always returns the same function. That is why a handler assigned through a proxy can be removed: [BX24.unbind](./bx24-unbind.md) receives the same function as [BX24.bind](./bx24-bind.md).

The method works on an application page where the [BX24.js library](../index.md) is connected and does not call Bitrix24, so there is no need to wait for [BX24.init](../system-functions/bx24-init.md). The method requires no scope of its own.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **func*** 
[`callable`](../../../api-reference/data-types.md) | The original function: declared with `function` or as an object method. For arrow functions and functions bound with `Function.prototype.bind`, `this` is not replaced ||
|| **thisObject*** 
[`object`](../../../api-reference/data-types.md) | The object that `this` refers to inside `func` ||
|#

## Code Example

{% include [Note on examples](../../../_includes/examples.md) %}

Assign an object method as the button handler and remove it after the third click:

```html
<button id="counter">Click</button>

<script>
    BX24.ready(function () {
        const counter = {
            clicks: 0,
            onClick: function () {
                this.clicks++;
                console.log('Clicks:', this.clicks);

                if (this.clicks === 3) {
                    BX24.unbind(button, 'click', BX24.proxy(this.onClick, this));
                }
            }
        };

        const button = document.getElementById('counter');
        BX24.bind(button, 'click', BX24.proxy(counter.onClick, counter));
    });
</script>
```

## Response Handling

The method synchronously returns a result of type `function`. The proxy function passes all of its arguments to `func` and returns the result of `func`.

### Returned Data

#| 
|| **Name**
`type` | **Description** ||
|| **result** 
[`function`](../../../api-reference/data-types.md) | A proxy function that calls `func` with `this` set to `thisObject` ||
|#

A repeated call with the same pair of `func` and `thisObject` returns the same function:

```js
const counter = { onClick: function () {} };

BX24.proxy(counter.onClick, counter) === BX24.proxy(counter.onClick, counter); // true
```

## Error Handling

The method does not return error codes.

#|
|| **Situation** | **What Happens** | **What to Do** ||
|| `thisObject` or `func` is not passed or is `null`, `0`, `false`, or an empty string | The method returns `func` as is, without a proxy | Pass both parameters ||
|| A non-empty string is passed in `func` | The method throws a `TypeError` exception | Pass a function ||
|| An object rather than a function is passed in `func` | The method returns a proxy function, but a `TypeError` exception occurs when it is called | Pass a function ||
|| Something other than an object is passed in `thisObject`, for example, the string `'text'` or the number `5` | The method throws a `TypeError` exception | Pass an object ||
|| `thisObject` inherits via `Object.create` from an object with which the method has already been called for the same `func` | The method returns the proxy function of the parent object: `this` inside `func` refers to the parent, and there is no error | Pass an object created without inheriting from an object that has already been passed ||
|| `func` or `thisObject` is non-extensible when first passed to the method — after `Object.freeze`, `Object.seal`, or `Object.preventExtensions` | The method throws a `TypeError` exception: the library cannot write an internal property to them | Pass a regular object and function ||
|| An exception occurs inside `func` | The exception propagates out of the proxy function. After that, [BX24.proxyContext](./bx24-proxy-context.md) may return the context of the interrupted call | Catch exceptions inside `func` ||
|#

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./bx24-proxy-context.md)
- [{#T}](./bx24-bind.md)