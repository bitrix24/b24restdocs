# Check User Administrator Access with BX24.isAdmin

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The method `BX24.isAdmin` determines whether the current user has administrator rights in Bitrix24. This method works after [BX24.init](../system-functions/bx24-init.md).

```js
Boolean BX24.isAdmin()
```

## Parameters

No parameters.

## Code Example

{% include [Example Footnote](../../../_includes/examples.md) %}

```js
BX24.init(function () {
    const isAdmin = BX24.isAdmin();
    console.log(isAdmin);
});
```

## Response Handling

The method synchronously returns a result of type `boolean`.

### Returned Data

#|  
|| **Name**  
`type` | **Description** ||  
|| **result**  
[`boolean`](../../../api-reference/data-types.md) | `true` if the current user has administrator rights in Bitrix24, otherwise `false` ||  
|#

## Continue Learning

- [{#T}](../system-functions/bx24-init.md)  
- [{#T}](../../../api-reference/common/users/user-admin.md)  