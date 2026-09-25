# Call a Bitrix24 Method BX24.callMethod

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

```js
void BX24.callMethod(
    String method,
    Object params[,
        Function callback
    ]
);
```

The `BX24.callMethod` function calls a Bitrix24 method on behalf of the user who opened the application. The library automatically adds authorization data to the request and converts the **params** object into a POST request string.

Values in **params** can be strings, numbers, arrays, nested objects, and dates. The library passes a date as a string in ISO 8601 format. Instead of a value, you can pass a form field element: for a regular field, the library takes its value, and for an `<input type="file">` field, the selected file.

The function returns nothing: the result is passed to the `callback` function. If you call `BX24.callMethod` before [BX24.init](../system-functions/bx24-init.md), the library defers the request until initialization completes.

## Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **method***
[`string`](../../../api-reference/data-types.md) | Bitrix24 method name, for example, [user.get](../../../api-reference/user/user-get.md) ||
|| **params***
[`object`](../../../api-reference/data-types.md) | Parameters of the called method. They are listed on the page of that method. If the method has no parameters, pass an empty object `{}`: the `callback` function must be the third argument ||
|| **callback**
[`function`](../../../api-reference/data-types.md) | Function that receives the request result — an [ajaxResult](#ajax-result) object. Without it, the request is executed, but you cannot find out the result ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

Retrieve the user with ID `10`. The application needs one of the following scopes for the [user.get](../../../api-reference/user/user-get.md) method: `user`, `user_brief`, or `user_basic`:

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

Retrieve all users page by page. The [user.get](../../../api-reference/user/user-get.md) method returns up to 50 records per call. While `result.more()` returns `true`, the `result.next()` function requests the next page and passes it to the same handler. You cannot pass `start` in `params`: the library removes this parameter from the request.

```js
BX24.init(() => {
    BX24.callMethod('user.get', { sort: 'ID', order: 'ASC' }, (result) => {
        if (result.error())
        {
            console.error(result.error().toString());
            return;
        }

        console.log(result.data());
        if (result.more())
        {
            result.next();
        }
    });
});
```

Upload an employee photo from a form field using the [user.update](../../../api-reference/user/user-update.md) method. The library reads the selected file and passes it in the `PERSONAL_PHOTO` parameter. The method requires the `user` scope, and only an administrator can change another employee's profile:

```js
// <input type="file" id="photo"> — a field on the application page
BX24.init(() => {
    BX24.callMethod('user.update', {
        ID: 10,
        PERSONAL_PHOTO: document.getElementById('photo'),
    }, (result) => {
        if (result.error())
        {
            console.error(result.error().toString());
            return;
        }

        console.log(result.data()); // true
    });
});
```

## Response Handling {#ajax-result}

The `callback` function receives an `ajaxResult` object. Its `answer` property stores the raw Bitrix24 response, for example, for the [user.get](../../../api-reference/user/user-get.md) method:

```json
{
    "result": [
        {
            "ID": "10",
            "ACTIVE": true,
            "NAME": "Klaus",
            "LAST_NAME": "Weber",
            "UF_DEPARTMENT": [1]
        }
    ],
    "total": 1,
    "time": {
        "start": 1790283114,
        "finish": 1790283114.0253,
        "duration": 0.0253,
        "processing": 0,
        "date_start": "2026-09-24T20:51:54+00:00",
        "date_finish": "2026-09-24T20:51:54+00:00"
    }
}
```

The object's methods make the response easier to read.

### ajaxResult Object Methods

#|
|| **Method** | **What it returns** ||
|| `data()` | The `result` field of the response: an array, an object, or a scalar value, depending on the called method. If an error occurred, it returns `undefined` ||
|| `error()` | An error object, or `undefined` if there is no error. The structure of the error object is described in the [Error Handling](#errors) section ||
|| `error_description()` | The error message, or `undefined` if there is no error ||
|| `more()` | `true` if the list has a next page ||
|| `total()` | The total number of records in the list. If the method did not return it, `total()` returns `NaN` ||
|| `next(cb)` | Requests the next page of the list and passes it to the `cb` function or, if it is not provided, to the original handler. If there are no more pages, it returns `false` ||
|#

In addition to methods, the object has properties: `answer` — the raw Bitrix24 response, `status` — the HTTP status of the response, `query` — a copy of the request settings with the method name, parameters, and `callback`. No separate method returns the request execution time: it is stored in `result.answer.time`, and its fields are described in the [Time Object](../../../api-reference/data-types.md#time) section.

## Error Handling {#errors}

The library passes an error returned by Bitrix24 to `callback`. The `result.error()` method returns an object:

```json
{
    "status": 401,
    "ex": {
        "error": "insufficient_scope",
        "error_description": "The request requires higher privileges than provided by the access token"
    }
}
```

The error code is available in `result.error().ex.error`, and the message in `result.error().ex.error_description`. The `getError()` method returns the entire `ex` object, the `result.error().status` property and the `getStatus()` method return the HTTP status, and the `toString()` method returns a formatted string containing the code, message, and status.

Error codes depend on the called method and are described on its page.

{% include notitle [error handling](../../../_includes/error-info.md) %}

The following are not passed to `callback`:

- a response with a `5xx` status, a network failure, and a response that could not be parsed as JSON. The library throws a `Query error!` exception from the asynchronous request handler, so a `try/catch` around the call does not catch it
- the `expired_token` error. The library refreshes the authorization itself and retries the request, and `callback` receives the result of the retry

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue your exploration

- [{#T}](./index.md)
- [{#T}](./bx24-call-batch.md)
- [{#T}](./bx24-call-bind.md)
- [{#T}](./bx24-call-unbind.md)