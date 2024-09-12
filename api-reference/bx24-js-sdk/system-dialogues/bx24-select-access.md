# Show Access Permission Selection Dialog BX24.selectAccess

```js
BX24.selectAccess(value: array, callback: callable): void;
BX24.selectAccess(callback: callable): void;
```

The function `BX24.selectAccess` displays a standard access permission selection dialog.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **value**
[`array`](../../data-types.md) | "Blocked" access permissions that should not be selected (non-starting value) ||
|| **callback***
[`callable`](../../data-types.md) | Callback function.

The `callback` handler will receive an array of objects in the form `{id: string, name: string}`, where: 
- `id` — access permission identifier. Examples of identifiers:
    - **U1** — user with identifier 1
    - **SG4** — social network group with identifier 4
    - **AU** — all authorized users
- `name` — name of the access permission ||
|#

## Code Example

```js
BX24.selectAccess(
    function(params)
    {
        for (var i in params)
        {
            let param = params[i];
            BX('name' + i).value = param.name;
            BX('id'  + i).value = param.id;
        }
    }
)
```

{% include [Note on examples](../../../_includes/examples.md) %}

## Continue Learning

- [{#T}](./bx24-select-user.md)
- [{#T}](./bx24-select-users.md)
- [{#T}](./bx24-select-crm.md)