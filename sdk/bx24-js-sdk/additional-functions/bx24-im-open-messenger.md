# Open Messenger Window BX24.im.openMessenger

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

{% include notitle [iframe context](../../../_includes/app-runs-in-iframe.md) %}

```js
BX24.im.openMessenger(dialogId?: string): void;
```

The `BX24.im.openMessenger` method opens the chat list or a selected dialog in Bitrix24 Messenger.

The method works after [BX24.init](../system-functions/bx24-init.md).

## Method Parameters

#| 
|| **Name**
`type` | **Description** ||
|| **dialogId**
[`string`](../../../api-reference/data-types.md) | Dialog identifier. Supported formats:

- user identifier, for example, `42`
- chat identifier in the `chatXXX` format, for example, `chat123`
- group chat identifier in the `sgXXX` format, for example, `sg456`
- Open Channel dialog identifier in the `imol|XXXX` format, for example, `imol|1234`

If the parameter is not passed, the method opens the chat list ||
|#

{% note info "" %}

The method passes `dialogId` to Messenger without validation or conversion.

{% endnote %}

## Code Example

{% include [Example Note](../../../_includes/examples.md) %}

```js
BX24.init(function () {
    BX24.im.openMessenger('chat123');
});
```

## Response Handling

The method sends a command to open Messenger and returns nothing.

## Error Handling

The method does not pass error codes to the application. If `dialogId` does not match an available dialog, the application does not receive a description of the cause.

## Continue Learning

- [{#T}](./bx24-im-open-history.md)
- [{#T}](./bx24-im-call-to.md)
- [{#T}](./bx24-open-path.md)
