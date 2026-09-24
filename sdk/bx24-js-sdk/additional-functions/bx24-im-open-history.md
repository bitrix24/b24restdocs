# Open History Window BX24.im.openHistory

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

{% include notitle [iframe context](../../../_includes/app-runs-in-iframe.md) %}

```js
BX24.im.openHistory(dialogId: string): void;
```

The `BX24.im.openHistory` method passes a dialog identifier to Bitrix24 Messenger and opens the message history window.

The method works after [BX24.init](../system-functions/bx24-init.md).

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#| 
|| **Name** 
`type` | **Description** ||
|| **dialogId*** 
[`string`](../../../api-reference/data-types.md) | Dialog identifier. Supported formats:

- user identifier, for example, `42`
- chat identifier in the `chatXXX` format, for example, `chat123`
- Open Channel dialog identifier in the `imol\|XXXX` format, for example, `imol\|1234` ||
|#

{% note info "" %}

The method passes `dialogId` to Messenger without validation or conversion.

{% endnote %}

## Code Example

{% include [Note on examples](../../../_includes/examples.md) %}

```js
BX24.init(function () {
    BX24.im.openHistory('chat123');
});
```

## Response Handling

The method sends a command to open the history and returns nothing.

## Error Handling

The method does not pass error codes to the application. If `dialogId` does not match an available dialog, the application does not receive a description of the cause.

## Continue Learning

- [{#T}](./bx24-im-open-messenger.md)
- [{#T}](./bx24-im-call-to.md)
- [{#T}](./bx24-open-path.md)
