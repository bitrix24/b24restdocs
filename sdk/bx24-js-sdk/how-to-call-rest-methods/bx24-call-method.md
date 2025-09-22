# Call the REST service method with specified parameters BX24.callMethod

## Sending a request

```js
void BX24.callMethod(
    String method,
    Object params[,
        Function callback
    ]
);
```

The method `BX24.callMethod` invokes the specified REST service method with the given parameters. The parameters of the method are represented by the **params** object, which is an associative array converted into a POST request string. The values of the array elements can be strings or references to DOM elements of form fields (see file uploads below).

If called before [BX24.init](../system-functions/bx24-init.md), the request execution will be delayed.

## Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **method***
[`string`](../../../api-reference/data-types.md) | The name of the REST API method to be called ||
|| **params**
[`object`](../../../api-reference/data-types.md) | Parameters passed to the method. Must match the expected parameters of the called method ||
|| **callback**
[`function`](../../../api-reference/data-types.md) | The callback function that will be executed after receiving a response from the server ||
|#

## Example

```js
BX24.init(() => {
    BX24.callMethod('user.get', { ID: 10 }, (result) => {
        if (result.error())
        {
            console.error(result.error());
        }
        else if (result.data())
        {
            const user = result.data()[0];
            if (user)
            {
                alert('User №' + user.ID + ' is named ' + user.NAME);
            }
        }
    });
});
```

{% include [Note on examples](../../../_includes/examples.md) %}

{% note warning %}

In on-premise versions, REST methods are called using the method `BX.rest.callMethod()`, not `BX24.callMethod()`.

{% endnote %}

## Handling the request result {#ajax-result}

The result handler of the request is a function. It receives an **ajaxResult** object, which provides the following methods:

- `data()` — returns the response data of the method. The type of returned data can be an array, object, or scalar value, depending on the specific API method  
- `error()` — returns an error object if an error is present, and `undefined` otherwise. The error object contains the fields `status` and `ex`, where `ex` includes `error` and `error_description`
- `more()` — returns `true` if there is more data to load, and `false` otherwise. Applicable if the method returns a list of data
- `total()` — returns the total number of available records. Applicable if the method returns a list of data
- `time()` — returns an object with information about the request execution time. It may return `undefined` if the information is unavailable.
- `next(cb: Function)` — requests the next page of data. If a function `cb` is provided, it will be used as the result handler for the next request. Returns `false` if there is no more data to load, or an `XMLHttpRequest` object if the request was initiated.

## Example of retrieving a paginated list of users

```js
BX24.init(() => {
    BX24.callMethod('user.get', { sort: 'ID', order: 'ASC' }, (result) => {
        if (result.error())
        {
            alert('Request error: ' + result.error());
        }
        else
        {
            console.log(result.data());
            if (result.more())
            {
                result.next();
            }
        }
    });
});
```

{% include [Note on examples](../../../_includes/examples.md) %}

## Continue your exploration

- [{#T}](./bx24-call-bind.md)
- [{#T}](./bx24-call-unbind.md)
- [{#T}](./bx24-call-batch.md)