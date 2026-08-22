# Open History Window BX24.im.openHistory

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

{% include notitle [iframe context](../../../_includes/app-runs-in-iframe.md) %}

The method `BX24.im.openHistory` sends a command to open the history window of a dialog.

```js
void BX24.im.openHistory(String dialogId)
```

## Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#| 
|| **Name** 
`type` | **Description** ||
|| **dialogId*** 
`string` | Identifier of the dialog. Supported formats: `userId` or `chatXXX` for chat, ```imol|XXXX``` for Open Channels ||
|#

## Code Example

{% include [Note on examples](../../../_includes/examples.md) %}

```js
BX24.init(function () {
    BX24.im.openHistory('chat123');
});
```

## Response Handling

The method does not return any data (`void`).

## Continue Learning

- [{#T}](./bx24-im-open-messenger.md)
- [{#T}](./bx24-im-call-to.md)
- [{#T}](./bx24-open-path.md)
