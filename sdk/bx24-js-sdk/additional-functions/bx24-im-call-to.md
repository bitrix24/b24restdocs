# Call via Internal Communication BX24.im.callTo

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

{% include notitle [iframe context](../../../_includes/app-runs-in-iframe.md) %}

```js
BX24.im.callTo(userId: integer, video?: boolean): void;
```

The `BX24.im.callTo` method passes a user identifier to Bitrix24 Messenger and starts an internal call.

The method works after [BX24.init](../system-functions/bx24-init.md).

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#| 
|| **Name** 
`type` | **Description** ||
|| **userId*** 
[`integer`](../../../api-reference/data-types.md) | Identifier of the Bitrix24 user to call ||
|| **video** 
[`boolean`](../../../api-reference/data-types.md) | Call format. Possible values:

- `true` — video call
- `false` — audio call

Default — `false` ||
|#

## Code Example

{% include [Note on examples](../../../_includes/examples.md) %}

```js
BX24.init(function () {
    BX24.im.callTo(42, true);
});
```

## Response Handling

The method sends a call command and returns nothing.

## Error Handling

The method does not pass error codes to the application. If the call cannot be started, the application does not receive a description of the cause.

## Continue Learning

- [{#T}](./bx24-im-phone-to.md)
- [{#T}](./bx24-im-open-messenger.md)
