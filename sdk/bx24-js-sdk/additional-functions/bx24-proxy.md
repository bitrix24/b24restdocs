# Get the BX24.proxy Function

The `BX24.proxy` method creates a proxy function for calling `func` in the context of `thisObject`. This method is similar to `BX.proxy`. When called again with the same `func` and `thisObject`, it returns the same proxy function.

```js
Function BX24.proxy(Function func, Object thisObject)
```

## Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **func*** 
`function` | The original function for which the proxy is created ||
|| **thisObject*** 
`object` | The object that will be used as `this` when calling `func` ||
|#

## Code Example

{% include [Note on examples](../../../_includes/examples.md) %}

```js
BX24.init(function () {
    const context = {
        value: 10,
        print: function (step) {
            console.log(this.value + step);
        }
    };

    const proxy = BX24.proxy(context.print, context);
    proxy(5); // 15
});
```

## Response Handling

The method synchronously returns a result of type `function`.

### Returned Data

#| 
|| **Name**
`type` | **Description** ||
|| **result** 
`function` | Proxy function for calling `func` in the context of `thisObject` ||
|#

## Continue Learning

- [{#T}](./bx24-proxy-context.md)
- [{#T}](./bx24-bind.md)