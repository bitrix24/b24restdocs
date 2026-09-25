# Send a batch of requests with BX24.callBatch

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

The `BX24.callBatch` function sends several Bitrix24 method calls in a single request. A batch is useful when you need to perform a series of calls in a row: for example, to create the required objects during application installation, or to retrieve a user together with their department.

```js
void BX24.callBatch(
    Object|Array calls,
    [Function callback[,
    Boolean bHaltOnError = false]]
);
```

The function returns nothing: the results of all commands are passed to the `callback` function. If you call `BX24.callBatch` before [BX24.init](../system-functions/bx24-init.md), the library defers the request until initialization completes.

## Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **calls***
[`object`](../../../api-reference/data-types.md)\|[`array`](../../../api-reference/data-types.md) | Batch commands — an array or an object. Each command is an array `[method_name, method_parameters]` or an object `{method: method_name, params: method_parameters}`. The object keys become the result keys in `callback`.

In command parameters, you can refer to the result of a previous command using the `$result[command_key][response_field]` macro, for example, `$result[get_user][UF_DEPARTMENT]`.

Bitrix24 does not execute commands beyond the 50th. The library does not send an empty batch, and `callback` is not called ||
|| **callback**
[`function`](../../../api-reference/data-types.md) | Function that receives the command results — an array or an object of [ajaxResult](./bx24-call-method.md#ajax-result) objects with the same keys as in `calls` ||
|| **bHaltOnError**
[`boolean`](../../../api-reference/data-types.md) | Whether to stop the batch at the first error. Commands after the error are not executed, and their keys are absent from the result. Default is `false`: Bitrix24 executes all commands ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

Retrieve the current user using the [user.current](../../../api-reference/user/user-current.md) method and, in the same batch, their departments using the [department.get](../../../api-reference/departments/department-get.md) method. The second command takes the department IDs from the response of the first one using a macro. The application needs the `department` scope and one of the scopes for user data: `user`, `user_brief`, or `user_basic`:

```js
BX24.init(() => {
    BX24.callBatch({
        get_user: ['user.current', {}],
        get_department: {
            method: 'department.get',
            params: {
                ID: '$result[get_user][UF_DEPARTMENT]',
            },
        },
    }, (result) => {
        for (const key of ['get_user', 'get_department'])
        {
            // with bHaltOnError = true, commands after an error are absent from the result
            if (!result[key])
            {
                console.error(key + ': command was not executed');
                return;
            }

            if (result[key].error())
            {
                console.error(key + ': ' + result[key].error().toString());
                return;
            }
        }

        const user = result.get_user.data();
        const departments = result.get_department.data().map((department) => department.NAME);
        console.log(user.NAME + ' ' + user.LAST_NAME + ': ' + departments.join(', ')); // Klaus Weber: Sales Department
    }, true);
});
```

## Response Handling {#response}

Bitrix24 returns a single response to the batch, in which the data, errors, and metadata of each command are grouped by key. The raw response to the batch from the example above:

```json
{
    "result": {
        "result": {
            "get_user": {
                "ID": "10",
                "ACTIVE": true,
                "NAME": "Klaus",
                "LAST_NAME": "Weber",
                "UF_DEPARTMENT": [1]
            },
            "get_department": [
                {
                    "ID": "1",
                    "NAME": "Sales Department",
                    "SORT": 500
                }
            ]
        },
        "result_error": [],
        "result_total": {
            "get_department": 1
        },
        "result_next": [],
        "result_time": {
            "get_user": {
                "start": 1790289763,
                "finish": 1790289763.9619,
                "duration": 0.9619,
                "processing": 0,
                "date_start": "2026-09-24T22:42:43+00:00",
                "date_finish": "2026-09-24T22:42:43+00:00"
            },
            "get_department": {
                "start": 1790289763,
                "finish": 1790289763.9637,
                "duration": 0.9637,
                "processing": 0,
                "date_start": "2026-09-24T22:42:43+00:00",
                "date_finish": "2026-09-24T22:42:43+00:00"
            }
        }
    },
    "time": {
        "start": 1790289763,
        "finish": 1790289763.9651,
        "duration": 0.9651,
        "processing": 0,
        "date_start": "2026-09-24T22:42:43+00:00",
        "date_finish": "2026-09-24T22:42:43+00:00"
    }
}
```

The library splits this response by command: `callback` receives results with the same keys as in `calls`. Each result is an [ajaxResult](./bx24-call-method.md#ajax-result) object: the `data()` method returns the command data, and the `error()` method returns the error. For example, `result.get_department.data()` returns the array from the `result.result.get_department` field.

A command result differs from a [BX24.callMethod](./bx24-call-method.md#ajax-result) result:

- for a command with an error, `data()` returns an empty object `{}` rather than `undefined`
- `error_description()` always returns `undefined`. The error message is in `result[key].error().ex.error_description`
- the `answer` property does not contain the request execution time
- `next()` requests the next page with a separate method call, outside the batch. Without an argument, it passes the page to the batch `callback` as a single result rather than as an object with keys, so pass a separate handler to `next()`
- if the command parameters contain a `$result` macro, `next()` sends it as is: Bitrix24 substitutes macros only inside a batch. For such a command, request the next page with a new batch of one command, with resolved parameter values and the `start` parameter. This does not work with [BX24.callMethod](./bx24-call-method.md): the library removes the `start` parameter from `params`

## Error Handling {#errors}

An error is returned for each command separately. If `bHaltOnError` is not enabled, the remaining commands of the batch are executed. The `result[key].error()` method returns the same error object as [BX24.callMethod](./bx24-call-method.md#errors). The HTTP status in it is the status of the entire batch, so for a command error it is usually `200`:

```json
{
    "status": 200,
    "ex": {
        "error": "ERROR_METHOD_NOT_FOUND",
        "error_description": "Method not found!"
    }
}
```

If `bHaltOnError` is enabled, the keys of commands after the error are absent from the result. Check that the key exists before calling `error()`: otherwise, the handler stops with a JavaScript error.

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

Command error codes depend on the called method and are described on its page. Errors of the batch itself:

#|
|| **Status** | **Code** | **Description** | **Meaning** ||
|| `200` | `ERROR_BATCH_LENGTH_EXCEEDED` | Max batch length exceeded | The batch contains more than 50 commands. If `bHaltOnError` is not enabled, every command beyond the limit receives this error; otherwise, only the 51st command does ||
|| `200` | `ERROR_BATCH_METHOD_NOT_ALLOWED` | Method is not allowed for batch usage | The batch contains a method that cannot be called in a batch, for example, a nested `batch` ||
|#

An error for the entire batch is not passed to `callback`:

- if Bitrix24 returns an error for the entire batch rather than for an individual command, the library stops with a JavaScript error while parsing the response
- for a response with a `5xx` status, a network failure, and a response that could not be parsed as JSON, the library throws a `Query error!` exception from the asynchronous request handler, so a `try/catch` around the call does not catch it

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./bx24-call-method.md)
- [{#T}](./bx24-call-bind.md)
- [{#T}](./bx24-call-unbind.md)