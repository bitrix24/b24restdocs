# Call the Phone Number BX24.im.phoneTo

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

{% include notitle [iframe context](../../../_includes/app-runs-in-iframe.md) %}

The method `BX24.im.phoneTo` sends a command to make a call to a phone number.

```js
void BX24.im.phoneTo(String phone)
```

## Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#| 
|| **Name** 
`type` | **Description** ||
|| **phone*** 
`string` | The phone number to call. International format is recommended (e.g., `+14151234567`). You can also pass the number in local format, such as `84012112233` or `8 (495) 711-22-33`. In the SDK, the value is passed as a string without additional validation ||
|#

## Code Example

{% include [Note on examples](../../../_includes/examples.md) %}

```js
BX24.init(function () {
    BX24.im.phoneTo('+14151234567');
});
```

## Response Handling

The method does not return data (`void`).

## Continue Learning

- [{#T}](./bx24-im-call-to.md)
- [{#T}](./bx24-im-open-messenger.md)