# Open Messenger Window BX24.im.openMessenger

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

{% include notitle [iframe context](../../../_includes/app-runs-in-iframe.md) %}

The method `BX24.im.openMessenger` sends a command to open the messenger window.

```js
void BX24.im.openMessenger([String dialogId])
```

## Parameters

#| 
|| **Name**
`type` | **Description** ||
|| **dialogId**
`string` | Identifier of the dialog. Supported formats: `userId` or `chatXXX` for chat, `sgXXX` for group chat, ```imol|XXXX``` for Open Channels. If the parameter is not provided, the chat list interface will open. ||
|#

## Code Example

{% include [Example Note](../../../_includes/examples.md) %}

```js
BX24.init(function () {
    BX24.im.openMessenger('chat123');
});
```

## Response Handling

The method does not return data (`void`).

## Continue Learning

- [{#T}](./bx24-im-open-history.md)
- [{#T}](./bx24-im-call-to.md)
- [{#T}](./bx24-open-path.md)
