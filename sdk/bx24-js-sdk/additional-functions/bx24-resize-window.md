# Change Frame Size with BX24.resizeWindow

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The method `BX24.resizeWindow` sends a command to change the size of the application frame based on the provided width and height.

```js
void BX24.resizeWindow(Integer width, Integer height[, Function callback])
```

## Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#| 
|| **Name** 
`type` | **Description** ||
|| **width*** 
`integer` | The width of the frame in pixels. Inside the method, the value is parsed using `parseInt` and is only used if greater than `0` ||
|| **height*** 
`integer` | The height of the frame in pixels. Inside the method, the value is parsed using `parseInt` and is only used if greater than `0` ||
|| **callback** 
`function` | The callback function that is executed after the resize command is sent ||
|#

## Code Example

{% include [Note on examples](../../../_includes/examples.md) %}

```js
BX24.init(function () {
    BX24.resizeWindow(980, 700, function () {
        console.log('Resize command sent');
    });
});
```

## Response Handling

The method does not return any data (`void`).

## Continue Learning

- [{#T}](./bx24-fit-window.md)
- [{#T}](./bx24-get-scroll-size.md)