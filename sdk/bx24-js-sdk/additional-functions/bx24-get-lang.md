# Get Language Identifier in Current Bitrix24 BX24.getLang

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The method `BX24.getLang` returns the language identifier in the current Bitrix24. This method works after [BX24.init](../system-functions/bx24-init.md).

```js
String BX24.getLang()
```

## Parameters

No parameters.

## Code Example

{% include [Example Notes](../../../_includes/examples.md) %}

```js
BX24.init(function () {
    const lang = BX24.getLang();
    BX24.loadScript('lang/' + lang + '.js', function () {
        console.log('Loading completed');
    });
});
```

## Response Handling

The method synchronously returns a result of type `string`.

### Returned Data

#| 
|| **Name**
`type` | **Description** ||
|| **result**
[`string`](../../../api-reference/data-types.md) | Language identifier in the current Bitrix24: `ja`, `id`, `ms`, `de`, `la`, `fr`, `it`, `pl`, `br`, `vn`, `tr`, `kz`, `de`, `en`, `ua`, `ar`, `th`, `sc`, `tc` ||
|#

## Continue Learning

- [{#T}](../system-functions/bx24-init.md)
- [{#T}](./bx24-is-admin.md)