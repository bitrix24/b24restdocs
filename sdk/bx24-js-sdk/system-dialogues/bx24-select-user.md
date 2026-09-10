# Show User Single Selection Dialog BX24.selectUser

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

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

The `callback` handler will receive an object of the selected employee — with the same fields as the elements of the array in [BX24.selectUsers](./bx24-select-users.md#callback):
- `id` — identifier of the employee, arrives as a string
- `name` — formatted name of the employee ||
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