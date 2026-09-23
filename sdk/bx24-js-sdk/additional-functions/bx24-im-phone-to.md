# Call the Phone Number BX24.im.phoneTo

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

{% include notitle [iframe context](../../../_includes/app-runs-in-iframe.md) %}

```js
BX24.im.phoneTo(string phone): void;
```

The `BX24.im.phoneTo` method passes a phone number to Bitrix24 Messenger and starts an outgoing call.

The method works after [BX24.init](../system-functions/bx24-init.md). Telephony must be available in Bitrix24 to make a call.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#| 
|| **Name** 
`type` | **Description** ||
|| **phone*** 
[`string`](../../../api-reference/data-types.md) | Phone number to call. Pass the number in international format, for example, `+4915112345678`. The method passes the string to Messenger without validating or converting the format ||
|#

## Code Example

{% include [Note on examples](../../../_includes/examples.md) %}

```js
BX24.init(function () {
    BX24.im.phoneTo('+14151234567');
});
```

## Response Handling

The method sends a call command and returns nothing.

## Error Handling

The method does not pass error codes to the application. If calls are unavailable on the current plan, Bitrix24 opens a window with information about the restriction.

## Continue Learning

- [{#T}](./bx24-im-call-to.md)
- [{#T}](./bx24-im-open-messenger.md)
