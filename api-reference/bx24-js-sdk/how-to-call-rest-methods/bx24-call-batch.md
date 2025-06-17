# Send a batch of requests with BX24.callBatch

In some cases, there is a need to send multiple requests in succession. For example, when creating necessary entities during the application installation process. To optimize the process, batch execution of requests can be used.

```js
void BX24.callBatch(
    Object|Array calls,
    [Function callback[,
    Boolean bHaltOnError = false]]
);
```

The function `BX24.callBatch` sends a batch of requests to the REST service. If called before [BX24.init](../system-functions/bx24-init.md), the request execution will be delayed.

## Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **calls***
[`object`](../../data-types.md)\|[`array`](../../data-types.md) | A regular or associative array (object) with requests. Each request is either an array `[method_name, method_parameters]` or an object `{method: method_name, params: method_parameters}`. 

In the method parameters, macros can be used to access the results of previous requests in the current batch. A macro can be constructed like this: `$result[request_identifier][response_field]`, where the request identifier is its key in the request batch array ||
|| **callback**
[`function`](../../data-types.md) | A handler function for the batch request result. It will receive an array or associative array (object) of [ajaxResult](./bx24-call-method.md#ajax-result) objects with keys corresponding to the keys from the request batch ||
|| **bHaltOnError**
[`boolean`](../../data-types.md) | Flag "halt execution of the batch in case of an error". Default is `false` (do not halt) ||
|#

## Example

```js
BX24.init(() => {
    const prepareMessage = (name, lastName, departmentNumber) => {
        return `The current user ${name} ${lastName} is assigned to the departmen${departmentNumber > 1 ? 'ts ' : 't '}`;
    };
    BX24.callBatch({
        get_user: ['user.current', {}],
        get_department: {
            method: 'department.get',
            params: {
                ID: '$result[get_user][UF_DEPARTMENT]',
            },
        },
    }, (result) => {
        if (result.get_user.error() || result.get_department.error())
        {
            if (result.get_user.error())
            {
                console.error(result.get_user.error());
            }

            if (result.get_department.error())
            {
                console.error(result.get_department.error());
            }
        }
        else
        {
            const departmentNumber = result.get_department.data().length;
            let message = prepareMessage(result.get_user.data().NAME, result.get_user.data().LAST_NAME, departmentNumber);

            for (let i = 0; i < departmentNumber; i++)
            {
                message += i === 0 ? '' : ', ';
                message += result.get_department.data()[i].NAME;
            }

            alert(message);
        }
    }, true);
});
```

{% include [Note on examples](../../../_includes/examples.md) %}

## Continue Learning

- [{#T}](./bx24-call-bind.md)
- [{#T}](./bx24-call-unbind.md)
- [{#T}](./bx24-call-method.md)