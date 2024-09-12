# Show Multiple User Selection Dialog BX24.selectUsers

```js
BX24.selectUsers(callback: callable): void;
```

The `BX24.selectUsers` method displays the standard multiple user selection dialog. It only shows company employees.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **callback***
[`callable`](../../data-types.md) | Callback function.

The `callback` handler will receive an array of objects in the format `{id: integer, name: string}`, where: 
- `id` — user identifier
- `name` — formatted user name ||
|#

## Code Example

```js

BX24.selectUsers(
    function(params)
    {
        for (var i in params)
        {
            let param = params[i];
            BX('student' + i).value = param.name;
            BX('student_external_id'  + i).value = param.id;
        }
    }
)
```

{% include [Note on examples](../../../_includes/examples.md) %}

## Continue Learning

- [{#T}](./bx24-select-user.md)
- [{#T}](./bx24-select-access.md)
- [{#T}](./bx24-select-crm.md)