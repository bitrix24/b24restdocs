# Get App Configurations BX24.appOption.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

```js
BX24.appOption.get(string name): mixed;
```

The `BX24.appOption.get` method returns a configuration by its code.

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../../api-reference/data-types.md) | Parameter code ||
|#

## Code Examples

```js
BX24.init(() => {
    BX24.appOption.set('param_str', 'str1', (params) => console.log(params));
    BX24.appOption.set('param_numb', 1);

    console.log(BX24.appOption.get('param_str')); //returns str1
    console.log(BX24.appOption.get('param_numb'));//returns 1
});
```

{% include [Note on examples](../../../_includes/examples.md) %}

## Return Value

Returns the value of the app configuration with the name `name`. The value type depends on what was retained by the [BX24.appOption.set](./bx24-app-option-set.md) method.

## Continue Learning

- [{#T}](./bx24-user-option-set.md)
- [{#T}](./bx24-user-option-get.md)
- [{#T}](./bx24-app-option-set.md)
