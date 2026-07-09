# Call the Registered Interface Command BX24.placement.call

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`placement`](../../scopes/permissions.md)

The method `BX24.placement.call` invokes a registered interface command.

```js
BX24.placement.call(command, parameters[, callback]);
```

## Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **command***
[`string`](../../data-types.md) | The command to be invoked ||
|| **parameters**
[`any`](../../data-types.md) | Passed parameters. The value type depends on the command: object, string, number, array, or `null` ||
|| **callback**
[`callable`](../../data-types.md) | Optional callback function ||
|#

For example, the command `setValue` at the `USERFIELD_TYPE` embedding point accepts a new value for the second parameter of the field:

```js
BX24.placement.call('setValue', value, () => {});
```

## Code Example

{% include [Note on examples](../../../_includes/examples.md) %}

```js
BX24.ready(function () {
    BX24.init(function () {
        BX24.placement.call('getStatus', {}, function (result) {
            console.log(result);
        });

        BX24.placement.call('CallCardSetCardTitle', {title: 'hello world!'}, function (result) {
            console.log(result);
        });
    });
});
```

## Continue Learning

- [{#T}](bx24-placement-info.md)
- [{#T}](bx24-placement-get-interface.md)
- [{#T}](bx24-placement-bind-event.md)
