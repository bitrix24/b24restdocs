# Start a Call to the Phone Number Messenger.startPhoneCall

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The method `Messenger.startPhoneCall` initiates a call to a phone number in Bitrix24. It is recommended to use this method instead of `BX24.im.phoneTo`.

```js
Promise Messenger.startPhoneCall(String number[, Object params])
```

## Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **number***
`string` | Phone number to call ||
|| **params**
`object` | Additional call parameters. The parameters object is passed further to the phone manager ||
|#

## Code Example

{% include [Note on examples](../../../_includes/examples.md) %}

```js
BX.Messenger.Public.startPhoneCall('88000000000');
```

## Handling the Response

The method returns a `Promise`.

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
`Promise` | Promise of the operation to initiate the call ||
|#

## Continue Your Learning

- [{#T}](./messenger-start-video-call.md)
- [{#T}](./outdated/bx24-im-phone-to.md)