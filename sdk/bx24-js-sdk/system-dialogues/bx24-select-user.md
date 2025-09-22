# Show User Single Selection Dialog BX24.selectUser

```js
BX24.selectUser(callback: callable): void;
```

The `BX24.selectUser` method displays the standard single user selection dialog.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **callback***
[`callable`](../../../api-reference/data-types.md) | Callback function.

The `callback` handler will receive an object of the form `{id: integer, name: string}`, where: 
- `id` — user identifier
- `name` — formatted user name ||
|#

## Code Example

```js
BX24.selectUser(
    function(params)
    {
        BX('student').value = params.name;
        BX('student_external_id').value = params.id;
    }
)
```

{% include [Note on examples](../../../_includes/examples.md) %}

## Continue Learning

- [{#T}](./bx24-select-users.md)
- [{#T}](./bx24-select-access.md)
- [{#T}](./bx24-select-crm.md)