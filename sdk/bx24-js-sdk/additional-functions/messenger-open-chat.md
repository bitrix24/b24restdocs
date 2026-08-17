# Open Chat Messenger.openChat

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The method `Messenger.openChat` opens a chat in the Bitrix24 Messenger interface. It is recommended to use this method instead of `BX24.im.openMessenger` and `BX24.im.openHistory`.

```js
Promise Messenger.openChat([String dialogId[, Integer messageId]])
```

## Parameters

#| 
|| **Name**
`type` | **Description** ||
|| **dialogId**
`string` | Identifier of the dialog or chat. If this parameter is not provided, the chat list will open. ||
|| **messageId**
`integer` | Identifier of the message to open the chat focused on a specific message. ||
|#

## Code Example

{% include [Example Notes](../../../_includes/examples.md) %}

The `Messenger` object becomes available after the `im.public.iframe` extension is loaded:

```js
BX.Runtime.loadExtension('im.public.iframe').then(function (exports) {
    exports.Messenger.openChat('chat123');
});
```

To open a chat focused on a specific message, pass the second parameter:

```js
BX.Runtime.loadExtension('im.public.iframe').then(function (exports) {
    exports.Messenger.openChat('chat123', 12345);
});
```

## Response Handling

The method returns a `Promise`.

### Returned Data

#| 
|| **Name**
`type` | **Description** ||
|| **result**
`Promise` | Promise of the chat opening operation. ||
|#

## Continue Learning

- [{#T}](./messenger-start-video-call.md)
- [{#T}](./messenger-start-phone-call.md)
- [{#T}](./bx24-open-path.md)