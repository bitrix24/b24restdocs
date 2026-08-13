# Get User Settings BX24.userOption.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

```js
BX24.userOption.get(string name): mixed;
```

The `BX24.userOption.get` method returns the value of the setting named `name` for the current user.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../../api-reference/data-types.md) | Parameter code ||
|#

## Code Examples

```js
BX24.init(() => {
    BX24.userOption.set('param_str', 'str');
    BX24.userOption.set('param_numb', 1);
    BX24.userOption.set('param_obj', {foo: 'bar'});

    console.log(BX24.userOption.get('param_str')); //returns str
    console.log(BX24.userOption.get('param_numb')); //returns 1
    console.log(BX24.userOption.get('param_obj')); //returns {foo: 'bar'}
});
```

{% include [Note on examples](../../../_includes/examples.md) %}

## Return Value

Returns the value of the setting named `name`. The value type depends on what was retained by the [BX24.userOption.set](./bx24-user-option-set.md) method.

## Continue Learning

- [{#T}](./bx24-user-option-set.md)
- [{#T}](./bx24-app-option-set.md)
- [{#T}](./bx24-app-option-get.md)
